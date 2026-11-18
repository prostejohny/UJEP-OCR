# OCR – získávání dat z textů, dokumentů a PDF

Zápočtový projekt do předmětu **Nástroje datové analýzy a prezentace dat**.
Repozitář obsahuje praktické ukázky zpracování souborů pomocí Pythonu a OCR a potřebné cvičné materiály.
Cílem je předvést rozdíl mezi digitálním textem a obrazovým skenem, extrakci dat z dokumentů, rozpoznávání českého textu a vyhodnocení kvality OCR.

## Co projekt ukazuje

- **Text a kódování:** UTF-8, Windows-1250 a ISO-8859-2; proč může čeština při špatném dekódování vypadat nesprávně.
- **Word (`.docx`):** čtení obsahu, stylů a tabulek prostřednictvím knihovny `python-docx`.
- **Digitální PDF:** získávání textové vrstvy a tabulek pomocí `PyMuPDF` a `pdfplumber`.
- **Skenované a smíšené PDF:** rozlišení stran obsahujících textovou vrstvu a obrazové skeny.
- **OCR:** rozpoznávání českého textu nástrojem Tesseract, porovnání českého a anglického modelu a zobrazení jistoty rozpoznání.
- **Vyhodnocení přesnosti:** metriky CER (chyby znaků) a WER (chyby slov) vůči referenčnímu textu.
- **Předzpracování skenu:** narovnání, zvětšení, vyhlazení a adaptivní prahování pomocí OpenCV.

## Struktura praktických materiálů

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── OCR_demo_colab.ipynb
└── data/
    ├── cvicne_dokumenty/
    │   ├── 01_zapis_z_porady.pdf
    │   ├── 01_zapis_z_porady_REFERENCNI_TEXT.txt
    │   ├── 02_faktura_tabulka.pdf
    │   ├── 03_sken_obycejny.pdf
    │   ├── 03_sken_obycejny.png
    │   ├── 04_sken_spatny.png
    │   ├── 05_smisene.pdf
    │   ├── 06_smlouva.docx
    │   ├── 07_kodovani_cp1250.txt
    │   ├── 07_kodovani_iso8859-2.txt
    │   └── 07_kodovani_utf8.txt
    └── cvicne_dokumenty.zip
```

## Spuštění praktické části v Google Colabu (doporučeno)

1. Na GitHubu otevřete soubor **[`notebooks/OCR_demo_colab.ipynb`](notebooks/OCR_demo_colab.ipynb)** a zvolte jeho otevření v Google Colabu (případně v Colabu použijte **Soubor → Otevřít poznámkový blok → GitHub**).
2. Spusťte první instalační buňku notebooku. Nainstaluje Tesseract, český jazykový model a potřebné knihovny.
3. Při spuštění další buňky se zobrazí výzva k nahrání souboru. Vyberte **[`data/cvicne_dokumenty.zip`](data/cvicne_dokumenty.zip)** z tohoto repozitáře; do Colabu ho nahrajte jako soubor z počítače.
4. Postupně spusťte zbývající buňky (`Shift + Enter` nebo **Runtime → Run all**). Budou pracovat se soubory z rozbaleného ZIPu.

V Colabu není potřeba ručně instalovat Python ani Tesseract na vlastní počítač. Notebook obsahuje ukázkové výstupy z předchozího spuštění; nové výpočty se mohou nepatrně lišit podle verze knihoven a Tesseractu.

## Lokální spuštění

Je potřeba **Python 3** a nainstalovaný **Tesseract OCR** s českým (`ces`) a anglickým (`eng`) jazykovým modelem. Pro otevření notebooku lze použít Jupyter Notebook nebo VS Code.

```bash
python -m venv .venv
source .venv/bin/activate     # macOS / Linux
python -m pip install -r requirements.txt
jupyter notebook
```

Na Windows se virtuální prostředí v PowerShellu aktivuje příkazem `.venv\Scripts\Activate.ps1`.
Tesseract musí být dostupný v systémové cestě (`tesseract --version`); v Linuxu lze balíčky obvykle nainstalovat například příkazem `sudo apt install tesseract-ocr tesseract-ocr-ces tesseract-ocr-eng`.

**Pozor:** První buňka s příkazy `apt-get` a `pip` je určená pro Google Colab. Při lokálním spuštění ji přeskočte. Notebook automaticky vyhledá složku `data/cvicne_dokumenty` jak při spuštění z kořene repozitáře, tak ze složky `notebooks`.

## Přehled cvičných dat

| Soubor | Účel |
|---|---|
| `01_zapis_z_porady.pdf` | PDF s textovou vrstvou; ukázka získání textu |
| `01_zapis_z_porady_REFERENCNI_TEXT.txt` | Referenční text pro výpočet chybovosti CER a WER |
| `02_faktura_tabulka.pdf` | Extrakce fakturační tabulky a kontrola výpočtů |
| `03_sken_obycejny.pdf`, `03_sken_obycejny.png` | Stejný obsah ve skenované podobě pro OCR |
| `04_sken_spatny.png` | Zhoršený sken pro demonstraci předzpracování |
| `05_smisene.pdf` | PDF pro ukázku rozpoznávání typů stran |
| `06_smlouva.docx` | Ukázka extrakce odstavců, stylů a tabulky z Wordu |
| `07_kodovani_*.txt` | Vzorky téhož textu v různých znakových kódováních |

Cvičné dokumenty slouží pro výuku. Vstupní soubory jsou v repozitáři ponechány i samostatně pro jednoduché prohlížení a stažení.

## Použité nástroje

- **Google Colab / Jupyter Notebook** – interaktivní prostředí pro praktickou ukázku
- **Python** (`pandas`, `numpy`, `Pillow`, `chardet`) – práce s daty a soubory
- **PyMuPDF, pdfplumber, python-docx** – čtení PDF a Word dokumentů
- **Tesseract + pytesseract** – rozpoznávání textu ze skenů
- **OpenCV** – úprava a předzpracování obrazů
- **RapidFuzz** – měření vzdálenosti mezi referenčním a rozpoznaným textem
- **Matplotlib** – grafy a vizualizace výsledků

## Poznámka k přesnosti OCR

OCR nemusí text převést bez chyb, zvláště při nízkém rozlišení, šumu, naklonění či neobvyklém rozložení stránky. **CER a WER jsou chybovosti, nikoli přímo procenta úspěšnosti.** Nižší hodnoty obvykle znamenají lepší výsledek. Automaticky získané tabulky a částky je vhodné ověřit proti originálu.
