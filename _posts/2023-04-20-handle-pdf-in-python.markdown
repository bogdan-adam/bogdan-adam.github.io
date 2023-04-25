---
layout: post
title: How to test PDF files in Python
date: 2023-04-20 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: pdf-files-python.jpg # Add image post (optional)
tags: [quality-engineering] # add tag
---

At some point in my career, I worked on an application that was handling multiple PDF files. Almost every end-to-end test that was created in
the automation framework performed some operations on a PDF file(downloading, counting pages, checking that specific text exists, formatting, font size).
Then I investigated different solutions on how we can test PDF files using Python 3, focusing mostly on checking font size and font name (this was a new feature that
was introduced, customers could specify before PDF generation the exact font size).

In order to test this new feature that was introduced I investigated two libraries **PyPDF2** and **PDFMiner**. After checking both solutions I decided to use [PDFMiner](https://github.com/pdfminer/pdfminer.six)
as it contained multiple operations on PDF files and it was easier to extend to cover other functionalities.

Using PDFMiner we can test PDF files:
- *extract text/images from PDF files (and perform assertions):*
{% highlight python %}
def extract_text_from_pdf(pdf_file_path):
    pdf_resource_manager = PDFResourceManager()
    text_from_pdf = StringIO()
    layout_params = LAParams()

    image_writer = ImageWriter('path-to-save-images-from-pdf/')
    pdf_text_converter = TextConverter(pdf_resource_manager, text_from_pdf,
                                      laparams=layout_params, imagewriter=image_writer)
    pdf_file = open(pdf_file_path, 'rb')
    pdf_page_interpreter = PDFPageInterpreter(pdf_resource_manager, pdf_text_converter)
    for current_page in PDFPage.get_pages(pdf_file, pagenos=set(), maxpages=0, password="",
                                         caching=True, check_extractable=True):
        pdf_page_interpreter.process_page(current_page)
    pdf_file.close()
    pdf_text_converter.close()
    return text_from_pdf.getvalue()
{% endhighlight %}

- *test font size and font name:*
{% highlight python %}
def get_fontsize_and_fontname_for_phrase(pdf_path, word, page_number):
    resource_manager = PDFResourceManager()
    layout_params = LAParams()
    device = PDFPageAggregator(resource_manager, laparams=layout_params)
    pdf_file = open(pdf_path, 'rb')
    pdf_page_interpreter = PDFPageInterpreter(resource_manager, device)
    global actual_font_size_pt, actual_font_name

    for current_page_number, page in enumerate(PDFPage.get_pages(pdf_file)):
        if current_page_number == int(page_number) - 1:
            logger.debug("Search for word: {} on page: {}".format(word, current_page_number))
            pdf_page_interpreter.process_page(page)
            layout = device.get_result()
            for textbox_element in layout:
                if isinstance(textbox_element, LTTextBox):
                    for line in textbox_element:
                        word_from_textbox = line.get_text().strip()
                        if word in word_from_textbox:
                            for char in line:
                                if isinstance(char, LTChar):
                                    # convert pixels to points
                                    actual_font_size_pt = int(char.size) * 72 / 96
                                    actual_font_name = char.fontname[7:]
    self.builtin.set_test_variable("${WORD_FONT_SIZE}", floor(actual_font_size_pt))
    self.builtin.set_test_variable("${WORD_FONT_NAME}", actual_font_name)
    pdf_file.close()
    device.close()
    return actual_font_size_pt, actual_font_name
{% endhighlight %}

I strongly recommend using PDFMiner for testing PDF files as it’s a maintained library and [well-documented](https://pdfminersix.readthedocs.io/en/latest/).