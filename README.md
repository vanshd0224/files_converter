import streamlit as st

from pdf2docx import Converter
from docx import Document
from reportlab.pdfgen import canvas
import tempfile




st.set_page_config(page_title = "File Converter " , page_icon = "↻" , layout = "wide" )
st.title("Simple pdf to word / Word to pdf converter ")
st.write(" Created by Vansh Gupta ")
tab1,tab2 = st.tabs(["Pdf --> Word " , "Word --> Pdf "])


with tab1:
    st.header("Pdf --> Word ")
    pdf_file = st.file_uploader("Upload your Pdf file which you want to convert " , type= ["pdf"] , key="pdf_to_word")
    if pdf_file:
        if st.button("convert to word"):
            with st.spinner("converting your pdf to word ....."):
                with tempfile.NameTemporaryFile(delete=False, suffix=" .pdf") as temp_pdf:
                    temp_pdf.write(pdf_file.read())
                    pdf_path=temp_pdf.name

            docx_path = pdf_path.replace(".pdf" , ".docx")
            cv = Converter (pdf_path)
            cv.convert(docx_path)

            cv.close()
            with open(docx_path , "rb") as f:
                st.success("Conversion Completed !!!")
                st.download_button(" Download your Word File " , f , file_name="jibbran_converted.docx")
with tab2:
    st.header("Word --> Pdf ")
    word_file = st.file_uploader("Upload your Word file which you want to convert " , type= ["docx"] , key="word_to_pdf")
    if word_file:
        if st.button("convert to pdf"):
            with st.spinner("converting your word to file ....."):
                with tempfile.NameTemporaryFile(delete=False, suffix=" .docx") as temp_word:
                    temp_docx.write(word_file.read())
                    docx_path=temp_docx.name

                pdf_path = docx_path.replace(".docx" , ".pdf")
                doc = document(docx_path)
                c = canvas.Canvas
                y = 800
                for p in doc.paragraphs:
                    text = p.text
                    if text.strip():
                        c.drawstring(50,y,text)
                        y -= 20
                        if y < 50 :
                            c.showpage()
                            y=800
                            c.save()
                            
                   
                    with open(pdf_path , "rb") as f:
                        st.success("Conversion Completed !!!")
                        st.download_button(" Download your Pdf File " , f , file_name="jibbran_converted.pdf")


                        st.markdown("_____")
                        st.caption("Made with ❤︎ using streamlit by VANSH GUPTA ")
