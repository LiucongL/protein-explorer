# Protein Explorer

A single-file web app for exploring one protein at a time: its UniProt annotations, Pfam matches, MobiDB disorder tracks, experimental (PDB) construct coverage and AlphaFold model, displayed along the sequence and in 3D.

Enter a gene name, protein name or UniProt accession, choose an organism, and press **Search**. Select a domain, region or residues to highlight them in the sequence, architecture diagram and 3D structure. Protein-specific links use query parameters such as `?q=KRAS&org=9606` or `?q=P01116`.

Current version: **v1.3.7**, also shown beside the app name in the header and in the footer.

- **Open the app:** **https://liucongl.github.io/protein-explorer/** — no account needed.
- **Source code and releases:** **https://github.com/liucongl/protein-explorer**
<!-- Institute download mirror: link shared privately on request. -->

## How it works

The app is plain HTML and JavaScript, hosted as a static page. It has no application backend of its own. Protein data are requested from public services when you search.

| Information | Source |
|---|---|
| Names, sequence, regions, motifs and cross-references | [UniProt](https://www.uniprot.org/) REST API |
| Pfam matches | [InterPro](https://www.ebi.ac.uk/interpro/) API: precomputed matches |
| Disorder tracks | [MobiDB](https://mobidb.org/) API |
| Predicted structure and confidence (pLDDT) | [AlphaFold DB](https://alphafold.ebi.ac.uk/) |
| Experimental structures | PDB records referenced by UniProt |
| 3D display | [3Dmol.js](https://3dmol.org/) |

The app does not require an account or maintain its own server-side search history. Searches and record requests are sent to the relevant data providers. Those providers and the website host may log requests; the browser may also retain history, cached data and preferences. Searches should not be considered private or anonymous.

## Reading the domain annotations

**Pfam matches are computational annotations, not experimentally measured functional boundaries.** Pfam builds profile hidden Markov models from curated seed alignments. These models are searched against protein sequences, and the Explorer retrieves the precomputed matches supplied by InterPro. The Explorer does not run HMMER or calculate the match boundaries itself.

The diagram uses the start and end coordinates returned by InterPro. These are described here as **match coordinates**: HMMER distinguishes alignment coordinates from envelope coordinates, and those terms should not be used interchangeably without checking the source data.

- A match identifies a region resembling the family's sequence model. Its boundaries may differ from an experimentally studied peptide or the region required for a particular activity. A match does not establish that every residue within it is functionally necessary.
- A match on a mouse protein is a computational annotation of that mouse sequence. It does not, by itself, mean the function or its boundaries were tested experimentally in mouse. UniProt region annotations are shown separately with their associated evidence.
- A region called a “domain” need not be an independently folded unit. This matters particularly for transactivation regions and other regions that can be disordered.
- Annotations can change as sequences, models and database releases are updated. For coordinates you rely on, record the protein accession and isoform, residue range, Pfam identifier, retrieval date and database release when available.

UniProt's own domain annotations (usually PROSITE profiles, which can span a longer region than the corresponding Pfam model — a KRAB domain, for example) are listed under **UniProt regions** with their evidence, so the two sets of boundaries can be compared on the same diagram. Each domain row links to its source record, and a short note at the foot of the domain list in the app points to this section. If InterPro is unavailable, the app falls back to UniProt domain annotations and labels that source accordingly.

## Other things to know

- **AlphaFold:** residue mapping is checked against the UniProt sequence before annotations are overlaid. If mapping fails, the model can still be displayed, but domain colouring and residue highlighting are disabled with an explanatory note. Partial models are labelled with their coverage. pLDDT describes prediction confidence; it is not a direct measurement of disorder.
- **Experimental structures:** distinguish the sequence represented by a deposited construct from residues actually resolved in its structure. Coverage does not mean every residue has coordinates. MobiDB missing-residue tracks provide a separate view of unresolved residues.
- **Disorder:** different prediction methods can disagree. An unmarked region does not establish that it is ordered; use the track's method and evidence labels when interpreting it.
- **Isoforms:** an isoform accession, such as `P01116-2`, selects that isoform's sequence, while domains, structures and disorder tracks refer to the canonical entry. Check the sequence and mapping labels before transferring coordinates between them.
- **Availability:** public services can be unavailable or return incomplete annotations. A failed request is not evidence that a protein lacks a domain, structure or disordered region.

## Running and hosting

Use the app from a static web host over HTTPS for reliable browser features such as clipboard access. A downloaded HTML file may also work in a desktop browser, but it still needs internet access for database requests. Local-file handling varies between browsers and mobile file viewers; the hosted link is the simplest option.

To update a hosted copy, replace `index.html` and redeploy it. Keep the version shown in the app, this README and any release metadata consistent.

## Reporting problems

**Issue tracker:** **https://github.com/liucongl/protein-explorer/issues**

Please include the app version, protein accession and organism, browser, steps to reproduce the problem, and what you expected to happen. Include the displayed error or a screenshot when useful.

## Citing

If you use this tool in your research, please cite the version used. Author, version and release details are kept in `CITATION.cff` in the repository; GitHub shows them under **Cite this repository**. 

Also cite the underlying databases according to their guidance, and identify the relevant protein accessions, structure IDs and database versions or access dates so the annotations can be traced.

---

I developed this application with Claude and used GPT to review the code.
