# Automatische Kwantificatie van Abdominaal Gas door CT-gebaseerde Orgaansegmentatie

## Beschrijving

Dit Python-script voert automatische kwantificatie en 3D-visualisatie van gas in de maag, dunne darm en het colon uit op basis van abdominale CT-scans. Gassegmentatie wordt uitgevoerd op basis van Hounsfield Units, gecombineerd met orgaansegmentatie via TotalSegmentator.

## Functionaliteit
- Segmentatie van gas door Hounsfield-Unit drempelwaarde
- Verwijdering van achtergrondlucht en kleine gasregio's
- Automatische segmentatie van maag, dunne darm en colon via TotalSegmentator
- Berekening van totaal abdominaal gasvolume en het gasvolume per orgaansegment
- 3D-visualisatie van abdominaal gas

## Vereisten
- Python 3.11.11+ (werkt mogelijk voor oudere versies, maar is ontwikkeld in 3.11.11)
- Visual Studio Code (voor API-integratie met TotalSegmentator)
- Python Packages:
    - numpy
    - pydicom
    - skimage
    - scipy.ndimage
    - nibabel
    - nilearn.image
    - pyvista
    - pandas
    - totalsegmentator

## Installatie van TotalSegmentator
Volg de [installatie-instructies](https://github.com/wasserth/TotalSegmentator) op de TotalSegmentator GitHub-pagina.

## Configuratie
Pas bovenin het script de volgende parameters aan:
```
DICOM_folder = "/pad/naar/dicom"
threshold = -550  
volume_threshold_ml = 0.03
```

Daarnaast kunnen de TotalSegmentator parameters aangepast worden. Deze bestaan uit:
```
quiet = True   # minder output in de terminal
fast = False   # snellere maar minder nauwkeurige segmentatie
robust_crop = True # langzamere maar nauwkeurigere cropping
roi_subset=["stomach", "small_bowel", "colon", "duodenum"]  # selectie aan organen
higher_order_resampling = True  # verfijndere segmentaties
```



## Gebruik
Voer het script uit in Visual Studio code of met:
```
python GasAnalyse_Final.py
```
De verwerking van één CT-scan duurt gemiddeld 7 minuten.

## Output
De mapstructuur ziet er als volgt uit:
<pre> 📁 GasAnalyse/
├── 📄 GasAnalyse_Final.py # Hoofdscript 
├── 📁 TotalSegmentator_Output/ # Output van orgaansegmentatie 
├── 📁 Kwantificatie_Output/  # Gasvolume-kwantificatie 
└── 📁 Gasmasker_Output/ # Binair gasmasker na filtering </pre>

In de Kwantificatie_Output map bevindt zich een CSV met de gasvolumes in milliliter na het uitvoeren van het script.
<br/>
In de TotalSegmentator_Output map bevindt zich de output van de orgaansegmentaties in NIfTI-formaat.
<br/>
In de Gasmasker_Output map bevindt zich het binaire gasmasker in NIfTI-formaat, na verwijdering van de achtergrondlucht en kleine gasregio's

## Beperkingen
- Kleine onderzoekspopulatie (n=19)
- Beperkte nauwkeurigheid van orgaansegmentatie bij afwijkende anatomie
- Geen validatie van orgaanspecifieke gaskwantificatie
- Alleen getest op datasets van Rijnstate Ziekenhuis Arnhem

## Toekomstige verbeteringen
- Anders aankleuren van niet toegewezen gasregio's
- Niet toegewezen gasregio's aan organen toekennen als er overlap is met slechts één orgaansegmentatie
- In- en uitschakelen van orgaansegmenten in 3D-visualisatie
- Totaalvolume organen bepalen 
- Totaalvolume per orgaan bepalen via `statistics`-functie van TotalSegmentator  
- Restvolume (feces/vloeistof) schatten door gasvolume af te trekken van orgaanvolume

