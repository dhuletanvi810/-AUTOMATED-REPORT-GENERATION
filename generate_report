import argparse
from pathlib import Path
import pandas as pd
from fpdf import FPDF

def read_data(file_path: str) -> pd.DataFrame:
    """Load CSV into a DataFrame."""
    return pd.read_csv(file_path)

def analyze_data(df: pd.DataFrame) -> dict:
    """Return summary statistics in a plain-Python dict."""
    summary = {"Total Rows": len(df), "Numeric Columns": {}}

    for col in df.select_dtypes("number"):
        summary["Numeric Columns"][col] = {
            "mean": df[col].mean(),
            "sum": df[col].sum(),
            "min": df[col].min(),
            "max": df[col].max(),
        }
    return summary

class PDFReport(FPDF):
    def header(self):
        self.set_font("Helvetica", "B", 16)
        self.cell(0, 10, "Automated Report", ln=True, align="C")
        self.ln(4)

    def footer(self):
        self.set_y(-15)
        self.set_font("Helvetica", "I", 8)
        self.cell(0, 10, f"Page {self.page_no()}", align="C")

def generate_pdf(summary: dict, output_path: str):
    """Create the PDF using FPDF."""
    pdf = PDFReport()
    pdf.add_page()
    pdf.set_font("Helvetica", "", 12)

    # Overall stats
    pdf.set_font("Helvetica", "B", 12)
    pdf.cell(0, 10, "Summary Statistics", ln=True)
    pdf.ln(2)

    pdf.set_font("Helvetica", "", 12)
    pdf.multi_cell(0, 8, f"Total Rows: {summary['Total Rows']}")
    pdf.ln(2)

    for col, stats in summary["Numeric Columns"].items():
        pdf.set_font("Helvetica", "B", 12)
        pdf.cell(0, 8, col, ln=True)
        pdf.set_font("Helvetica", "", 12)
        for name, val in stats.items():
            pdf.multi_cell(0, 8, f"  - {name.title()}: {val:.2f}")
        pdf.ln(1)

    pdf.output(output_path)

def main():
    import sys

    if len(sys.argv) == 1:
        sys.argv += ["sample_sales.csv", "-o", "output.pdf"]

    parser = argparse.ArgumentParser(description="Generate a PDF report from CSV data.")
    parser.add_argument("input_csv", help="Path to input CSV file")
    parser.add_argument("-o", "--output", default="report.pdf", help="Output PDF path")
    args = parser.parse_args()

    df = read_data(args.input_csv)
    summary = analyze_data(df)
    generate_pdf(summary, args.output)
    print(f"✅ Report saved to {Path(args.output).resolve()}")

if __name__ == "__main__":
    main()
