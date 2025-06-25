# AUTOMATED-REPORT-GENERATION

*COMPANY*: CODTECH IT SOLUTIONS

*NAME*: SUDHARSAN C

*INTERN ID*: CTO4DF1197

*DOMAIN*: PYTHON PROGRAMMING

*DURATIONS*: 4WEEKS

*MENTORS*: NEELA SANTHOSH

*DISCRIPTION*: 
1. User Interface Setup
The application begins with a neat UI layout. The page is set up using st.set_page_config with a centered layout and a custom page title ("Automated Report Generator"). The header and markdown messages guide the user on what to do: upload a CSV file, input their name, choose an internship end date, and click a button to generate a report.

2. User Input Section
There are three main user inputs:

Text Input: Where the user enters their full name. This will be shown on the generated report as the person receiving the certificate.

Date Input: Allows the user to select the end date of their internship. The selected date is later formatted into a human-readable format and printed on the PDF.

File Uploader: Accepts only CSV files. The application uses pandas.read_csv() to read the file contents into a DataFrame.

3. CSV File Processing and Display
Once a file is uploaded:

The app displays a success message confirming that the file was uploaded.

The full dataset is previewed using st.dataframe().

Then, a statistical summary of the dataset is generated using df.describe(include='all'), which provides a quick overview of numeric and non-numeric data, including count, unique values, top frequency values, and standard statistics.

4. PDF Report Generation
The main logic of the app lies in the generate_pdf_report() function:

A PDF is generated using the reportlab library.

The PDF starts with a title ("AUTOMATED DATA REPORT").

Below that, it includes:

The name of the intern (entered by the user)

The internship end date

Organization name (hardcoded as “CODTECH”)

Then it iteratively writes each column’s summary statistics into the PDF using a loop.

Page breaks are handled by checking vertical space (y position) and starting a new page if needed.

Finally, the current date and time are included as a timestamp at the bottom of the report.

The PDF is stored in an in-memory buffer (io.BytesIO) instead of writing it to disk.

5. Download Link for the PDF
To let the user download the generated PDF, the buffer content is encoded in base64. A download link is created using HTML <a> tag, which is rendered in the app using st.markdown() with unsafe_allow_html=True.

6. Error Handling
If the CSV file fails to load or parse correctly, an error message is displayed.

If no file is uploaded yet, an informational message prompts the user to upload one.
