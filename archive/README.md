# Archive

`original_run.ipynb` is the Google Colab notebook that produced the reported
results, kept with every printed output and figure from that run. It is a
record of what was run, not the version to re-run.

## What changed from the file as run

Nothing in the code or outputs, apart from:

- one note cell added at the top;
- three comment lines in the two neighbourhood-size cells reworded to remove a
  person's name.

## How the outputs map to the run

- **Step 5 and Step 6 appear twice.** The first copy of each builds and
  evaluates the neighbourhood block from the 10 nearest neighbours; the second
  copy, added later for the neighbourhood-size ablation, uses 3.
- **Steps 7–15 were run on the 10-neighbour features**, before the second
  copies were added. Step 8's printed macro-F1 of 0.8245 for tuned LightGBM on
  `genes_neighbors` is the value reported for that setting.
- **File paths** (`/content/drive/MyDrive/...`) refer to the Google Drive used
  for the run.
- **Cluster-to-label matching** in Steps 9, 11, 12 and 13 is the original
  rule described under "Correction to cluster-to-label matching (v1.1)" in the
  main README.

## To re-run

Use `notebooks/02_benchmark.ipynb`. Release **v1.0** reproduces the numbers
recorded here; the current version is v1.1.
