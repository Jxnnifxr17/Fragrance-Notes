# Fragrance-Notes
Scent Chords

An interactive chord diagram showing which fragrance notes get used together. 

Each arc on the wheel is a fragrance note, grouped by scent family. A ribbon between two arcs means those notes appear in the same perfume, and the thicker the ribbon, the more perfumes share them. Click any note to see its ranked list of partners.

<img width="568" height="567" alt="Screenshot 2026-10-21 at 7 00 09 PM" src="https://github.com/user-attachments/assets/da234bce-5439-4fa9-977c-ee11d799b5dd" />

What's in the data:  
Perfumes	24,063  
Distinct notes	1,671  
Notes on the wheel	80 most common  
Pairings stored	900 strongest  
Scent families	9 (Citrus, Floral, Fruity, Green & herbal, Spice, Woods & moss, Amber & sweet, Musk & clean, Resin & leather)  

The source is fra_cleaned.csv, a Fragrantica-derived dataset with one row per perfume. The columns used are Top, Middle and Base (comma-separated note lists). The file is semicolon-separated and not UTF-8, so it is read with sep=";" and encoding="latin-1".

Features:  
Connections dropdown: show the strongest 100, 250, 500 or 900 pairings. Notes with no remaining connections drop off the ring.  
Click an arc: fades unrelated ribbons and shows that note's top 12 partners, with the count of shared perfumes and the share of the note's perfumes.  
Pyramid position: shows how often a note is listed as a top, heart or base note.  
Hover a ribbon: names the pair and the number of perfumes behind it.  
Light and dark mode: follows the system setting.  

Files
File	Purpose
Fragrance.ipynb: Reads the CSV and builds the co-occurrence data
scent_chords_offline.html: Finished page with D3 built in; works without internet
data.json: The generated data (notes, families, pairs)
fra_cleaned.csv	Input dataset

Running it:  
Open scent_chords_offline.html in a regular web browser. Double-click it, or use this from Python:
python. 
import webbrowser, os  
webbrowser.open("file://" + os.path.abspath("scent_chords_offline.html")). 

Jupyter's file viewer and some editor previews turn JavaScript off, so the wheel will not draw there. Use a real browser.

Rebuilding the data

Requires Python 3 and pandas.


How it works:  
Parse notes. Each perfume's Top, Middle and Base lists are merged into one set of notes, and the pyramid position of every note is tallied.
Pick the wheel. The 80 most common notes are kept (K in the script).  
Assign families. Keyword rules sort notes into families, with a hand-edited OVERRIDE dictionary for notes the rules get wrong.  
Count co-occurrence. For every perfume, every pair of kept notes gets +1. The 900 strongest pairs are exported (MAX_PAIRS).  
Draw. The page uses D3's chord layout to draw arcs and ribbons from that data.  
Customizing. 
Number of notes: change K in build_scent_chords.py.  
Family assignments: edit KEYWORDS and OVERRIDE in the same file.  
Colors: edit the C array of family colors in the HTML template.  

Limitations:  
This shows how perfumes are described on Fragrantica, not how they smell. Note lists are written by brands and fans.  
Popular perfumes are overrepresented, and raw counts favor very common notes such as musk, bergamot and sandalwood.  
Family groupings are editorial choices, not a chemical classification.  
Only the top 80 notes appear, so rarer notes are not on the wheel.
