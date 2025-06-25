import streamlit as st
import pandas as pd
from reportlab.pdfgen import canvas
from reportlab.lib.pagesizes import A4
from datetime import datetime
import tempfile
import base64
import io

# Function to generate PDF from data summary
def generate_pdf_report(data_summary, intern_name="Intern", end_date="30 June 2025"):
    buffer = io.BytesIO()
    c = canvas.Canvas(buffer, pagesize=A4)
    width, height = A4

    # Title
    c.setFont("Helvetica-Bold", 20)
    c.drawCentredString(width / 2, height - 80, "AUTOMATED DATA REPORT")

    # Certificate note
    c.setFont("Helvetica", 12)
    c.drawString(50, height - 130, f"Completion certificate will be issued to: {intern_name}")
    c.drawString(50, height - 150, f"Internship End Date: {end_date}")
    c.drawString(50, height - 170, "Organization: CODTECH")

    # Data Summary Section
    c.setFont("Helvetica-Bold", 14)
    c.drawString(50, height - 210, "Data Summary:")

    c.setFont("Helvetica", 10)
    y = height - 230
    for col in data_summary.columns:
        c.drawString(60, y, f"{col} Summary:")
        y -= 14
        for idx, val in data_summary[col].items():
            val_str = str(round(val, 2)) if pd.notna(val) and isinstance(val, (int, float)) else str(val)
            c.drawString(80, y, f"{idx}: {val_str}")
            y -= 12
        y -= 10
        if y < 80:
            c.showPage()
            y = height - 100

    # Footer
    c.setFont("Helvetica-Oblique", 9)
    c.drawString(50, 30, f"Report generated on: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")

    c.save()
    buffer.seek(0)
    return buffer

# Function to generate download link for PDF
def get_pdf_download_link(pdf_buffer):
    b64 = base64.b64encode(pdf_buffer.read()).decode()
    href = f'<a href="data:application/pdf;base64,{b64}" download="report.pdf">📥 Download PDF Report</a>'
    return href

# ---------------------- Streamlit UI ----------------------

st.set_page_config(page_title="Automated Report Generator", layout="centered")
st.title("📊 Automated Report Generator")
st.markdown("Upload a **CSV file**, enter your name and internship end date, and download a **PDF report**.")

# User inputs
intern_name = st.text_input("🧑 Your Full Name", "Your Name")
end_date = st.date_input("📅 Internship End Date", value=datetime.today())
uploaded_file = st.file_uploader("📂 Upload CSV File", type=["csv"])

if uploaded_file is not None:
    try:
        df = pd.read_csv(uploaded_file)
        st.success("✅ File uploaded successfully!")
        st.subheader("📄 Data Preview")
        st.dataframe(df)

        st.subheader("📈 Summary Statistics")
        summary = df.describe(include='all')
        st.dataframe(summary)

        if st.button("📄 Generate PDF Report"):
            pdf_buffer = generate_pdf_report(summary, intern_name, end_date.strftime("%d %B %Y"))
            st.success("✅ PDF report generated!")
            st.markdown(get_pdf_download_link(pdf_buffer), unsafe_allow_html=True)

    except Exception as e:
        st.error(f"⚠️ Error reading the file: {e}")
else:
    st.info("👆 Please upload a CSV file to continue.")
