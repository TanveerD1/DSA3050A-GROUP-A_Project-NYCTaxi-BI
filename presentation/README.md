# NYC Taxi BI Project Presentation Guide

This folder contains the planning guide for the final presentation of the **DSA3050A Business Intelligence & Visualization** group project.

The presentation deck itself can be created in PowerPoint, Google Slides, or Canva. Power BI visuals will be added later from the report file in the [pbix](../pbix) folder.

## 1. Presentation Purpose

The goal of this presentation is to communicate:
- The business problem and why it matters.
- The data pipeline and modeling work completed.
- Key analytical findings from the NYC taxi dataset.
- Actionable recommendations for taxi operations.

## 2. Audience and Delivery Style

Primary audience:
- Course lecturer and evaluators.
- Classmates (mixed technical and non-technical background).

Recommended style:
- Business-focused and insight-driven.
- Keep technical details concise, but show enough evidence of BI process rigor.
- Use clear visuals and avoid crowded slides.

## 3. Project Context Summary

Project: **New York City Taxi Rides BI Analysis**  
Course: **DSA3050A - Business Intelligence & Data Visualization**

Repository structure used for presentation evidence:
- [README.md](../README.md): project overview, business questions, star schema intent.
- [documentation/README.md](../documentation/README.md): transformation and modeling process with screenshots.
- [data/new york city taxi rides](../data/new%20york%20city%20taxi%20rides): source CSV data files.
- [pbix/GROUP-A_Ny-Taxis.pbix](../pbix/GROUP-A_Ny-Taxis.pbix): Power BI report file for visuals and DAX output.

## 4. Recommended Slide Deck Structure

Use this as a baseline 12-15 slide flow.

1. Title Slide
- Project name, course, team members, submission date.

2. Executive Summary
- 3 to 5 high-level takeaways.
- One sentence on business value.

3. Problem Statement
- Operational pain points in NYC taxi demand/supply balancing.
- Why analytics is needed.

4. Business Questions
- Present the key questions from the project brief.
- Group questions by demand, revenue, and operations.

5. Data Source and Scope
- Kaggle source and citation.
- File list and data granularity.
- Coverage and assumptions.

6. Data Preparation Approach
- Power Query workflow.
- Cleaning and transformation categories (types, nulls, outliers, duplicates).

7. Data Modeling
- Star schema overview.
- Fact and dimension design rationale.

8. DAX and Calculated Logic
- Key measures and calculated columns (for example speed, time categories, date/time dimensions).
- Why each metric supports business questions.

9. Dashboard Walkthrough (Power BI)
- Main dashboard pages and intended user actions.
- Placeholder until final visuals are inserted.

10. Key Insights
- Demand peaks by time/day.
- Neighborhood performance differences.
- Payment behavior patterns.

11. Business Recommendations
- 3 to 5 practical, prioritized actions.
- Explain expected impact.

12. Limitations and Next Steps
- Data quality constraints and potential bias.
- Future enhancement ideas.

13. Conclusion
- Re-state outcomes and business value.

14. Q&A
- Backup slide prepared with additional charts.

## 5. Slide-by-Slide Content Template

Copy this template into your slide notes while building the deck.

### Slide 1: Title
- Include: project title, course code, team list.
- Speaker note: "This project analyzes NYC taxi trip behavior to identify demand patterns and operational opportunities."

### Slide 2: Executive Summary
- Include 3 concise bullets in this format:
	- Insight: [main finding]
	- Impact: [business implication]
	- Action: [recommended decision]

### Slide 3: Problem Statement
- Define the decision problem, not just the technical objective.
- Mention why timing, location, and trip behavior matter for utilization and revenue.

### Slide 4: Business Questions
- Reuse project questions and group them by theme:
	- Demand timing
	- Location performance
	- Fare/payment behavior

### Slide 5: Data Source
- Mention data files from the dataset folder.
- Include citation on slide footer.

### Slide 6: Data Preparation
- Summarize transformations from [documentation/README.md](../documentation/README.md):
	- Headers and data types fixed
	- Null and duplicate handling
	- Outlier filtering
	- Derived columns (for example speed and peak/off-peak labels)

### Slide 7: Data Model
- Present a simplified star schema graphic.
- Explain fact-to-dimension relationships in plain language.

### Slide 8: Measures and DAX
- List 4 to 6 key measures with business intent.
- Keep formulas in appendix if too long.

### Slide 9: Dashboard 1 (Demand View)
- Placeholder for Power BI visual screenshot.
- Explain what the audience should look for.

### Slide 10: Dashboard 2 (Location View)
- Placeholder for map/bar visuals.
- Highlight top and bottom neighborhoods.

### Slide 11: Dashboard 3 (Payment and Trip Behavior)
- Placeholder for payment mix and trip distance/time visuals.
- Link to operational implications.

### Slide 12: Recommendations
- Use action language:
	- "Increase fleet coverage in ..."
	- "Adjust driver shift focus to ..."
	- "Monitor payment channel performance in ..."

### Slide 13: Limitations and Next Steps
- Examples:
	- Limited timeframe in source extract.
	- Potential geolocation quality issues.
	- Add weather/event data in future model versions.

### Slide 14: Conclusion and Q&A
- One-slide recap and thank-you.

## 6. Placeholders for Future Power BI Visuals

Since Power BI visuals will be added later, use this naming convention now so updates are easy:

- `visual-01-demand-over-time.png`
- `visual-02-top-neighborhoods.png`
- `visual-03-payment-breakdown.png`
- `visual-04-trip-distance-vs-duration.png`

Suggested process:
1. Export visuals from the PBIX report at consistent aspect ratio.
2. Store exported images in a dedicated subfolder (for example, `presentation/assets/`).
3. Replace placeholder boxes in slides with final images.
4. Re-check readability for projector viewing.

## 7. Design and Professional Quality Checklist

Before submission, verify:
- A single consistent slide theme (fonts, color palette, chart style).
- Maximum one core message per slide.
- All charts have clear titles and readable axes.
- Labels and legends are not truncated.
- Key numbers are highlighted and interpreted.
- Every insight is paired with a business implication.
- Citation is present for external data.
- Grammar, spelling, and spacing are consistent.

## 8. Suggested Team Presentation Split

Example flow for a 4 to 6 person group:
- Presenter 1: Introduction, problem statement, business questions.
- Presenter 2: Data source, transformation workflow.
- Presenter 3: Data model, DAX/measures.
- Presenter 4: Dashboard walkthrough and findings.
- Presenter 5: Recommendations and limitations.
- Presenter 6: Conclusion and Q&A moderation.

## 9. Submission-Ready Deliverables

Keep the following artifacts up to date:
- Final slide deck (`.pptx` or equivalent).
- Final PBIX report in [pbix](../pbix).
- Updated process documentation in [documentation/README.md](../documentation/README.md).
- This presentation guide in [presentation/README.md](README.md).

## 10. Data Citation

Ramireddy, S. (2023). *New York City taxi rides* [Data set]. Kaggle.  
https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides

