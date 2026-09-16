# Protein Explorer

A single-file web page for looking at one protein at a time: its UniProt annotations, Pfam domains, MobiDB disorder tracks, experimental (PDB) construct coverage and the AlphaFold model, drawn along the sequence and in 3D.

Type a gene name, protein name or UniProt accession, choose an organism, and press Search. Click a domain, a region or residues to highlight them in the sequence, the architecture diagram and the structure. Links to a specific protein work directly, e.g. `?q=KRAS&org=9606` or `?q=P01116`.

Current version: **v1.3.5** (shown next to the name in the header). Canonical source: *(GitHub link)*. Institute copy: *(GitLab Pages link)*.

## How it works

The page is plain HTML/JavaScript with no server of its own. Everything it shows is fetched live from public services when you search:

| Panel | Source |
|---|---|
| Names, sequence, regions, motifs, cross-references | [UniProt](https://www.uniprot.org/) REST API |
| Pfam domains | [InterPro](https://www.ebi.ac.uk/interpro/) API (precomputed matches) |
| Disorder tracks | [MobiDB](https://mobidb.org/) API |
| Predicted structure and confidence (pLDDT) | [AlphaFold DB](https://alphafold.ebi.ac.uk/) |
| Experimental structures | PDB cross-references listed by UniProt |
| 3D viewer | [3Dmol.js](https://3dmol.org/) |

Nothing you search is stored anywhere; the only data leaving your browser are the queries to those services.

## Reading the domain annotations

Pfam domains are **family matches, not measured boundaries**. Pfam builds a profile hidden Markov model for each family from a curated seed alignment; InterPro runs those models against every UniProt sequence and stores where each model matches. The Explorer retrieves those stored matches — it does not run the analysis itself — and draws the match envelope as the domain. Three consequences:

- The start and end of a domain are where the sequence stops resembling the family model. They need not coincide with the region shown experimentally to carry the function, which is often shorter (a structured peptide) or differently placed. Each domain row links to its InterPro entry, where the family's literature reference and seed alignment are listed.
- A match on one species is a homology inference from the family, not an annotation of that protein. UniProt's own region annotations, shown separately, carry their evidence with them.
- Matches change between releases, because InterPro recomputes them when Pfam models or the search software are updated. Record the InterPro/Pfam release with any coordinates you rely on.

If InterPro cannot be reached, the page falls back to UniProt's own domain annotations and says so in the headings; the short note at the foot of the domain list changes accordingly.

## Other things to know

- The AlphaFold model is used only after its residues have been verified against the UniProt sequence; if that fails, the model is still shown but domain colouring and residue highlighting are switched off and a note explains why. Partial models are labelled with the residues they cover.
- PDB coverage means the stretch of sequence present in a deposited construct, in UniProt numbering — not that every residue in it is resolved. MobiDB's missing-residue tracks report unresolved residues.
- An isoform accession (e.g. `P01116-2`) shows that isoform's sequence; domains, structures and tracks remain those of the canonical entry.

## Running and hosting

Open the file from any static web host over HTTPS (needed for the clipboard and, in the companion Workbench, tab coordination). Opening it directly from a phone's Files app does not run it. For updates, replace the file; the version pill tells everyone which build they are on.

## Citing

Please cite the UniProt, InterPro/Pfam, MobiDB, AlphaFold DB and PDB entries you used, with their release dates, in addition to this tool: *(citation / DOI to be added)*.
