# AI_COLLEGE_ASSISTANT
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak, KeepTogether
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.enums import TA_CENTER
from reportlab.lib.units import cm
from reportlab.pdfbase.ttfonts import TTFont
from reportlab.pdfbase import pdfmetrics
import os

out = "/mnt/data/AI_College_Assistant_Project.pdf"

doc = SimpleDocTemplate(
    out, pagesize=A4,
    rightMargin=1.8*cm, leftMargin=1.8*cm,
    topMargin=1.7*cm, bottomMargin=1.7*cm
)

navy = colors.HexColor("#17324D")
blue = colors.HexColor("#2F6B9A")
light_blue = colors.HexColor("#EAF2F8")
light_gray = colors.HexColor("#F4F6F8")
dark = colors.HexColor("#263238")
green = colors.HexColor("#2D7D68")

styles = getSampleStyleSheet()
styles.add(ParagraphStyle(name="CoverTitle", parent=styles["Title"], fontName="Helvetica-Bold",
                          fontSize=25, leading=31, textColor=navy, alignment=TA_CENTER, spaceAfter=12))
styles.add(ParagraphStyle(name="CoverSub", parent=styles["Normal"], fontSize=12, leading=18,
                          textColor=blue, alignment=TA_CENTER, spaceAfter=12))
styles.add(ParagraphStyle(name="Section", parent=styles["Heading1"], fontSize=17, leading=22,
                          textColor=navy, spaceBefore=8, spaceAfter=9))
styles.add(ParagraphStyle(name="SubSection", parent=styles["Heading2"], fontSize=12, leading=16,
                          textColor=blue, spaceBefore=7, spaceAfter=5))
styles.add(ParagraphStyle(name="BodyCustom", parent=styles["BodyText"], fontSize=9.5, leading=14,
                          textColor=dark, spaceAfter=6))
styles.add(ParagraphStyle(name="SmallCustom", parent=styles["BodyText"], fontSize=8, leading=11,
                          textColor=dark, spaceAfter=4))
styles.add(ParagraphStyle(name="CodeCustom", parent=styles["Code"], fontName="Courier",
                          fontSize=8, leading=11, backColor=light_gray, borderPadding=7,
                          leftIndent=8, rightIndent=8, spaceBefore=4, spaceAfter=8))
styles.add(ParagraphStyle(name="Callout", parent=styles["BodyText"], fontSize=9, leading=13,
                          textColor=navy, backColor=light_blue, borderPadding=8, spaceBefore=5, spaceAfter=8))

P = lambda text, style="BodyCustom": Paragraph(text, styles[style])
story = []

# Cover
story += [
    Spacer(1, 2.4*cm),
    P("AI COLLEGE ASSISTANT", "CoverTitle"),
    P("Project Documentation", "CoverSub"),
    Spacer(1, 0.5*cm),
]
cover_table = Table([
    [P("<b>Project Type</b>", "SmallCustom"), P("AI-assisted student information chatbot", "BodyCustom")],
    [P("<b>Primary Purpose</b>", "SmallCustom"), P("Answer common questions about subjects, notes, timetables, and college information", "BodyCustom")],
    [P("<b>Knowledge Source</b>", "SmallCustom"), P("A defined, curated knowledge base", "BodyCustom")],
    [P("<b>Suggested Level</b>", "SmallCustom"), P("Beginner-friendly Python project", "BodyCustom")],
], colWidths=[4.0*cm, 11.0*cm])
cover_table.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,-1), light_blue),
    ("BOX", (0,0), (-1,-1), 0.8, colors.HexColor("#C7D8E6")),
    ("INNERGRID", (0,0), (-1,-1), 0.4, colors.white),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 9),
    ("RIGHTPADDING", (0,0), (-1,-1), 9),
    ("TOPPADDING", (0,0), (-1,-1), 8),
    ("BOTTOMPADDING", (0,0), (-1,-1), 5),
]))
story += [cover_table, Spacer(1, 1.0*cm),
          P("Prepared as a project report for learning, documentation, and GitHub portfolio use.", "CoverSub"),
          Spacer(1, 3.0*cm),
          P("<b>Note:</b> This document describes a proposed project design. Features and sample responses should be adapted to match the implementation you build.", "Callout"),
          PageBreak()]

# Overview
story += [
    P("1. Project Overview", "Section"),
    P("The AI College Assistant is a student-support chatbot designed to answer frequently asked college-related questions from a defined knowledge base. Students can ask about subjects, study notes, class timetables, campus services, and other approved college information. The assistant searches the available information and returns a concise response.", "BodyCustom"),
    P("The project can begin as a simple Python command-line application and later be extended into a web interface. To keep answers dependable, the assistant should answer from the information supplied to it and clearly say when a requested detail is not available.", "BodyCustom"),
    P("2. Problem Statement", "Section"),
    P("Students often need to check multiple documents or contact staff for routine information such as course topics, class timings, notes, room details, and college procedures. This can take time, especially when information is spread across several sources. A centralized assistant can make approved information easier to find.", "BodyCustom"),
    P("3. Objectives", "Section"),
]
objectives = [
    "Provide quick answers to common student questions.",
    "Organize subject details, notes links, timetables, and college FAQs in one knowledge base.",
    "Reduce repeated manual searching for routine information.",
    "Return understandable answers and guide students to official sources when appropriate.",
    "Avoid inventing details when the knowledge base does not contain an answer."
]
for x in objectives:
    story.append(P("• " + x, "BodyCustom"))

story += [
    P("4. Main Features", "Section"),
]
features = [
    [P("<b>Feature</b>", "SmallCustom"), P("<b>Description</b>", "SmallCustom")],
    [P("Subject information", "SmallCustom"), P("Course names, syllabus topics, subject codes, and faculty details when provided.", "SmallCustom")],
    [P("Notes and study resources", "SmallCustom"), P("Find approved notes, document names, and links stored in the knowledge base.", "SmallCustom")],
    [P("Timetable lookup", "SmallCustom"), P("Show class times, rooms, and days when accurate timetable data is available.", "SmallCustom")],
    [P("College FAQs", "SmallCustom"), P("Answer questions about departments, facilities, office hours, and procedures from approved entries.", "SmallCustom")],
    [P("Unknown-answer handling", "SmallCustom"), P("Tell the student when information is missing and suggest checking the official college source.", "SmallCustom")],
]
ft = Table(features, colWidths=[4.0*cm, 11.0*cm], repeatRows=1)
ft.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy),
    ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("BACKGROUND", (0,1), (-1,-1), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 7),
    ("RIGHTPADDING", (0,0), (-1,-1), 7),
    ("TOPPADDING", (0,0), (-1,-1), 6),
    ("BOTTOMPADDING", (0,0), (-1,-1), 6),
]))
story += [ft, PageBreak()]

# Architecture
story += [
    P("5. System Architecture", "Section"),
    P("The architecture below shows a simple retrieval-based design. The student enters a question, the application normalizes it, searches the defined knowledge base, and formats the best matching information as a response.", "BodyCustom"),
]
arch_data = [
    [P("<b>Student</b>", "BodyCustom")],
    [P("Enters a question about subjects, notes, timetable, or college services", "SmallCustom")],
    [P("↓", "CoverSub")],
    [P("<b>Chat / Input Interface</b>", "BodyCustom")],
    [P("Command line first; web UI can be added later", "SmallCustom")],
    [P("↓", "CoverSub")],
    [P("<b>Question Processing</b>", "BodyCustom")],
    [P("Clean text, identify keywords or category, and search relevant entries", "SmallCustom")],
    [P("↓", "CoverSub")],
    [P("<b>Defined Knowledge Base</b>", "BodyCustom")],
    [P("FAQ entries, subject data, notes links, timetable records, college information", "SmallCustom")],
    [P("↓", "CoverSub")],
    [P("<b>Response Generator</b>", "BodyCustom")],
    [P("Format matching information; say when no reliable answer is found", "SmallCustom")],
    [P("↓", "CoverSub")],
    [P("<b>Answer shown to student</b>", "BodyCustom")],
]
arch = Table(arch_data, colWidths=[14.5*cm])
arch.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,-1), colors.white),
    ("BOX", (0,0), (-1,-1), 0.7, colors.HexColor("#C7D8E6")),
    ("INNERGRID", (0,0), (-1,-1), 0.35, colors.HexColor("#E1E8EE")),
    ("BACKGROUND", (0,0), (-1,1), light_blue),
    ("BACKGROUND", (0,3), (-1,4), light_gray),
    ("BACKGROUND", (0,6), (-1,7), light_blue),
    ("BACKGROUND", (0,9), (-1,10), light_gray),
    ("BACKGROUND", (0,12), (-1,13), light_blue),
    ("BACKGROUND", (0,15), (-1,15), colors.HexColor("#E5F4EC")),
    ("ALIGN", (0,0), (-1,-1), "CENTER"),
    ("VALIGN", (0,0), (-1,-1), "MIDDLE"),
    ("LEFTPADDING", (0,0), (-1,-1), 8),
    ("RIGHTPADDING", (0,0), (-1,-1), 8),
    ("TOPPADDING", (0,0), (-1,-1), 5),
    ("BOTTOMPADDING", (0,0), (-1,-1), 5),
]))
story += [arch, Spacer(1, 8), P("6. Suggested Technology Stack", "Section")]
tech = [
    [P("<b>Component</b>", "SmallCustom"), P("<b>Suggested choice</b>", "SmallCustom"), P("<b>Purpose</b>", "SmallCustom")],
    [P("Programming language", "SmallCustom"), P("Python 3", "SmallCustom"), P("Core logic and question handling", "SmallCustom")],
    [P("Knowledge base", "SmallCustom"), P("JSON or CSV", "SmallCustom"), P("Store FAQs and college data in a simple editable format", "SmallCustom")],
    [P("Search", "SmallCustom"), P("Keyword matching; optional TF-IDF later", "SmallCustom"), P("Find relevant entries for a question", "SmallCustom")],
    [P("Interface", "SmallCustom"), P("Terminal initially; Flask/Streamlit later", "SmallCustom"), P("Let students ask questions", "SmallCustom")],
]
tt = Table(tech, colWidths=[3.4*cm, 5.2*cm, 6.4*cm], repeatRows=1)
tt.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy),
    ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 6),
    ("RIGHTPADDING", (0,0), (-1,-1), 6),
    ("TOPPADDING", (0,0), (-1,-1), 5),
    ("BOTTOMPADDING", (0,0), (-1,-1), 5),
]))
story += [tt, PageBreak()]

# Data model and sample interactions
story += [
    P("7. Knowledge Base Design", "Section"),
    P("Keep information structured so it can be updated without changing the main Python program. For a beginner version, a JSON file is a practical choice.", "BodyCustom"),
    P('''Example JSON record:''', "SubSection"),
    P('''{
  "category": "timetable",
  "question": "When is the first lecture?",
  "answer": "Check the latest official timetable for your section.",
  "source": "official_timetable.pdf",
  "last_updated": "YYYY-MM-DD"
}''', "CodeCustom"),
    P("Recommended fields include category, question or keywords, answer, source, and last_updated. Replace the example with verified college information before using it.", "Callout"),
    P("8. Example User Interaction", "Section"),
]
interaction = [
    [P("<b>Student question</b>", "SmallCustom"), P("<b>Assistant response (example)</b>", "SmallCustom")],
    [P("Where can I find notes for Mathematics?", "SmallCustom"), P("The knowledge base lists the approved Mathematics notes resource. Open the stored link or ask your department if the resource is missing.", "SmallCustom")],
    [P("What is my class timetable?", "SmallCustom"), P("Please provide your course and section, or check the latest official timetable. I can only show timetable data stored in the knowledge base.", "SmallCustom")],
    [P("What is the syllabus for a subject?", "SmallCustom"), P("I can show the syllabus topics available in the knowledge base. Please enter the subject name or code.", "SmallCustom")],
]
it = Table(interaction, colWidths=[5.0*cm, 10.0*cm], repeatRows=1)
it.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy),
    ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 7),
    ("RIGHTPADDING", (0,0), (-1,-1), 7),
    ("TOPPADDING", (0,0), (-1,-1), 6),
    ("BOTTOMPADDING", (0,0), (-1,-1), 6),
]))
story += [it, Spacer(1, 8), P("9. Implementation Plan", "Section")]
steps = [
    "Create a Python script that accepts a question from the user.",
    "Prepare a JSON or CSV knowledge base with verified FAQs, subjects, timetable details, and resource links.",
    "Normalize the question and search for the most relevant entry.",
    "Display the matching answer and its source when available.",
    "Add a clear fallback message for questions that cannot be answered.",
    "Test with common questions, spelling variations, missing data, and invalid input.",
    "Optionally add a web interface and administrative workflow for updating information."
]
for i, s in enumerate(steps, 1):
    story.append(P(f"{i}. {s}", "BodyCustom"))

story += [
    P("10. Privacy, Accuracy, and Limitations", "Section"),
    P("• Use approved, current college information and include sources or update dates where possible.<br/>• Do not place private student records, passwords, marks, or personal contact details in a public repository.<br/>• Restrict access if the project later handles non-public college information.<br/>• Timetables and procedures may change; direct students to official sources for confirmation.<br/>• A basic keyword-based assistant may misunderstand ambiguous questions and is not a substitute for official college staff.", "BodyCustom"),
    P("11. Testing Checklist", "Section"),
]
checks = [
    "Correct answers for known questions",
    "Helpful fallback when information is missing",
    "Works with empty input and extra spaces",
    "Handles different capitalization and common wording",
    "Sources and update dates are shown when available",
    "No secrets or private student data committed to GitHub"
]
for c in checks:
    story.append(P("☐ " + c, "BodyCustom"))

story += [
    Spacer(1, 8),
    P("12. Conclusion", "Section"),
    P("The AI College Assistant provides a practical starting point for a student-support chatbot. By keeping its knowledge in a defined, maintainable data source, the project can offer quick access to common academic and campus information while making uncertainty clear. It can be expanded gradually with better search, a graphical interface, and an approved process for updating content.", "BodyCustom"),
    Spacer(1, 10),
    P("Suggested repository files", "SubSection"),
    P("README.md • main.py • knowledge_base.json • requirements.txt • architecture.png • project_report.pdf", "Callout"),
]

def footer(canvas, doc):
    canvas.saveState()
    width, height = A4
    canvas.setStrokeColor(colors.HexColor("#D5DDE3"))
    canvas.line(1.8*cm, 1.25*cm, width-1.8*cm, 1.25*cm)
    canvas.setFont("Helvetica", 8)
    canvas.setFillColor(colors.HexColor("#667782"))
    canvas.drawString(1.8*cm, 0.85*cm, "AI College Assistant — Project Documentation")
    canvas.drawRightString(width-1.8*cm, 0.85*cm, f"Page {doc.page}")
    canvas.restoreState()

doc.build(story, onFirstPage=footer, onLaterPages=footer)

print(f"Created: {out}")
