# ATS CV Generator

A Claude Skill that turns your existing CV plus a target job description into an ATS-optimized, role-specific resume in DOCX format. Works in English and Latin American Spanish, with auto-detection based on the job description language.

## What it does

- Reads your existing CV (any format you can paste or upload) and a target job description.
- Auto-detects output language from the JD, or follows your explicit override.
- Restructures your experience into outcome-focused bullets aligned to the JD.
- Mirrors the JD's exact terminology where you have matching experience.
- Produces a single-column, ATS-parseable DOCX with standard section headings.
- Uses **only** information from your source CV. Will not fabricate metrics, achievements, or credentials.

## What it does NOT do

- Cover letters
- LinkedIn profile content
- PDF export (convert the DOCX yourself in Word, Google Docs, or any PDF tool)
- Live ATS scanning (you can test the output on [Resume Worded](https://resumeworded.com/resume-scanner) or [Kickresume](https://www.kickresume.com/en/ats-resume-checker/))

## Installation

### Option 1: Claude Skills (recommended)

1. Download or clone this repo.
2. Copy the `ats-cv-generator/` folder into your Claude skills directory (location depends on your Claude environment, typically `/mnt/skills/user/` or equivalent).
3. Start a new Claude session.
4. Trigger by asking: "Tailor my CV to this job description" with both your CV and the JD pasted in.

The skill auto-installs `python-docx` on first run, so no manual setup is needed if you're using the skill path.

### Option 2: Run the Python script directly

1. Clone the repo.
2. Install the dependency:
   ```bash
   pip install python-docx
   ```
3. Build a candidate data JSON file matching the schema in `examples/sample_data.json`.
4. Run:
   ```bash
   python scripts/build_cv.py --input my_data.json --output my_cv.docx --lang en
   ```

## Usage example

Once the skill is installed in your Claude environment, just paste your inputs in chat:

```
Here is my CV:
[paste your CV text, or attach a PDF/DOCX]

Here is the job description:
[paste the JD]

Please tailor my CV for this role.
```

The skill auto-triggers and produces a DOCX output. To override the auto-detected language:

```
Please tailor my CV in Spanish for this role.
```

or

```
Please tailor my CV in English for this role.
```

## Core rules the skill enforces

1. **No fabrication.** Only your verified experience. No invented metrics.
2. **No em dashes.** Period.
3. **Mirror JD terminology** exactly when you have matching experience.
4. **Single-column, ATS-safe formatting** only. No tables, no graphics, no icons.
5. **Outcome-focused bullets** using the formula `[Verb] [responsibility] -> [outcome] using [tool/method]`.
6. **Honest gaps.** Will not pad your CV to hide missing requirements. You will be told about any major gap so you can address it in your cover letter or application notes.

## File structure

```
ats-cv-generator/
├── SKILL.md                   # Main entry Claude reads when triggered
├── README.md                  # This file
├── references/
│   ├── ats_rules.md           # Structural and formatting rules
│   ├── voice_guide.md         # Bullet rewriting transformations
│   └── section_formulas.md    # Section-by-section templates
├── scripts/
│   └── build_cv.py            # Python DOCX generator (python-docx)
└── examples/
    └── sample_data.json       # Schema illustration with generic placeholders
```

## Customizing the skill

- **Add a new language:** extend the `LABELS` dict in `scripts/build_cv.py` with the section labels for that language, and add a parallel formula block in `references/section_formulas.md`.
- **Adjust formatting:** change font, sizes, or margins in `scripts/build_cv.py`. Keep the single-column, no-tables, no-graphics rules intact.
- **Change verb bank or transformations:** edit `references/voice_guide.md`. The skill reads it at runtime, so changes take effect immediately.

## License

MIT. Use it, modify it, ship it.

## Contributing

Pull requests welcome. Keep the no-fabrication rule and the single-column ATS rules intact, those are the load-bearing parts of the skill.
