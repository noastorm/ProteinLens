# ProteinLens
Alphafold protein folding front-end tool. Useful for early-stage protein target identification. Researcher could paste in a disease-linked protein and quickly see where drugs might bind.


ProteinLens — AlphaFold Binding Site & Disease Variant Explorer
A browser-based research tool for querying protein structure confidence, annotated binding residues, and disease-associated variants — built on live data from the UniProt REST API and AlphaFold Database.
No installation. No backend. Open index.html in any modern browser.

What it does
Enter a UniProt accession (e.g. P04637) or human gene name (e.g. TP53) and ProteinLens will:

Fetch the full canonical sequence and feature annotations from UniProt
Query the AlphaFold Database for a predicted structure and parse per-residue pLDDT confidence scores directly from the mmCIF file
Display the true pLDDT distribution (very high / confident / low / very low) across all residues
Extract experimentally annotated active sites, binding sites, and metal-binding residues from UniProt
Highlight binding and variant residues inline on the canonical sequence
Flag disease associations and list natural variants with pathogenicity labels (Pathogenic / Likely pathogenic / VUS / Benign)
Link directly to the entry in UniProt, AlphaFold DB, RCSB PDB, ClinVar, and Mol* 3D viewer


Scientific context
AlphaFold and pLDDT
AlphaFold 2 (DeepMind / EMBL-EBI) predicts protein 3D structures from sequence alone with high accuracy for well-ordered regions. Each residue in an AlphaFold model is assigned a pLDDT score (predicted local distance difference test, 0–100):
ScoreInterpretation≥ 90Very high confidence — reliable for structural analysis70–89Confident — generally trustworthy backbone50–69Low confidence — treat with caution, often flexible loops< 50Very low confidence — likely intrinsically disordered
ProteinLens parses pLDDT values directly from the AlphaFold mmCIF structure file (_ma_qa_metric_local.metric_value) rather than relying on summary statistics, giving a true per-residue distribution.

Note: Disordered regions (low pLDDT) are not automatically candidate binding pockets. Some are constitutively unstructured; others may fold upon binding. Interpretation requires additional context.

Binding site annotations
The binding site panel displays UniProt feature annotations — curated from experimental literature (X-ray crystallography, NMR, mutagenesis studies). These are not computational predictions. Types shown include:

Active site — catalytic residues directly involved in the reaction mechanism
Binding site — residues in contact with a ligand, substrate, or cofactor
Metal binding — residues coordinating metal ions (Zn²⁺, Fe²⁺, Ca²⁺, etc.)

Disease variant mapping
Natural variants are drawn from UniProt's curated variant annotations, which integrate data from sources including ClinVar, OMIM, and the literature. Pathogenicity labels reflect the annotation in UniProt at time of query. For clinical interpretation, always verify against the primary ClinVar record.

Data sources and provenance
DataSourceAccess methodProtein metadata, sequence, annotationsUniProt REST APIuniprotkb/{id}?format=jsonAlphaFold model availabilityAlphaFold EBI APIapi/prediction/{id}Per-residue pLDDTAlphaFold mmCIF structure fileParsed from AF-{id}-F1-model_v4.cif3D structure viewerMol* ViewerExternal link / embedded iframeVariant cross-referenceClinVar (NCBI)External link
All data is fetched live at query time. No data is stored or cached locally.

Limitations

Binding site panel shows UniProt annotations only — it does not run computational pocket prediction (e.g. FPocket, SiteMap, DoGSiteScorer). Proteins without experimental structural data may show no annotated sites even if druggable pockets exist.
pLDDT is not a druggability score. High confidence does not mean a pocket is ligandable; low confidence does not mean a region is unimportant.
Gene name search is restricted to reviewed human entries (Swiss-Prot, organism 9606). For non-human proteins, use the UniProt accession directly.
Variant table is capped at 20 entries for display. The full variant set is available on the linked UniProt page.
No offline mode — all panels require network access to the UniProt and AlphaFold APIs.


Suggested proteins to explore
GeneUniProtWhy it's interestingTP53P04637Most frequently mutated gene in cancer; extensive variant dataEGFRP00533Major oncology drug target; well-characterized binding sitesKRASP01116Long considered "undruggable"; recent covalent inhibitors (G12C)GCKP35557Glucokinase mutations cause MODY2 and persistent hyperinsulinismSARS-CoV-2 3CLproP0DTD1Protease target for antiviral drug design (nirmatrelvir/Paxlovid)BRCA1P38398Hereditary breast/ovarian cancer; large variant dataset

Roadmap / future directions
This tool is a frontend interface layer. Planned extensions include:

Embedded Mol viewer* with binding residues highlighted by color directly in the 3D structure
Computational pocket prediction via a Python/Flask backend running FPocket on the AlphaFold .cif file
Druggability scoring combining pLDDT confidence, pocket volume, and variant burden into a composite target score
Ligand input — accept a SMILES string and run AutoDock Vina server-side for docking pose prediction
Variant-on-structure overlay — map ClinVar pathogenic variants onto the 3D model and flag those within 5Å of annotated binding residues
Batch mode — query a list of UniProt IDs and export a comparative summary table


Background
This project grew out of an independent computational study on the dopamine transporter (DAT) using AlphaFold 3 and PyMOL, exploring structural predictions relevant to Parkinson's disease research. ProteinLens is intended as a lightweight, accessible interface for the early stages of structure-based target identification — bridging the gap between raw AlphaFold outputs and actionable insight for researchers without a dedicated bioinformatics pipeline.

Usage
1. Clone or download this repository
2. Open index.html in any modern browser (Chrome, Firefox, Safari, Edge)
3. Enter a UniProt accession or human gene name and click Analyze
No build step. No dependencies to install. No API keys required.

References
Jumper, J. et al. (2021). Highly accurate protein structure prediction with AlphaFold. Nature, 596, 583–589. https://doi.org/10.1038/s41586-021-03819-2
Varadi, M. et al. (2022). AlphaFold Protein Structure Database: massively expanding the structural coverage of protein-sequence space with high-accuracy models. Nucleic Acids Research, 50(D1), D439–D444. https://doi.org/10.1093/nar/gkab1061
The UniProt Consortium (2023). UniProt: the Universal Protein Knowledgebase in 2023. Nucleic Acids Research, 51(D1), D523–D531. https://doi.org/10.1093/nar/gkac1052

License
MIT License — free to use, modify, and distribute with attribution.
