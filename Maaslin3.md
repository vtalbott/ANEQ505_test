Load in data 
```
metadata <- read_tsv("../../03_metadata/processed_metadata/pempek_metadata_3_26.txt")

table <- read_tsv("pempek_table.txt")
taxonomy <- read_tsv("taxonomy.tsv")

taxonomy <- taxonomy %>%
  rename(feature = `Feature ID`)
```

Clean up table
```
# table
# 1. Move feature IDs to row names and transpose
feature_table <- table %>%
  rename(feature = `#OTU ID`) %>%
  column_to_rownames("feature") %>%
  as.matrix()

feature_table <- t(feature_table)
feature_table <- as.data.frame(feature_table, check.names = FALSE)
feature_table <- rownames_to_column(feature_table, var = "sampleid")
```

prepare data for maaslin3
```
#Keep only shared samples
common_ids <- intersect(feature_table$sampleid, metadata$sampleid)

feature_table <- feature_table %>%
  filter(sampleid %in% common_ids) %>%
  column_to_rownames("sampleid")

metadata <- metadata %>%
  filter(sampleid %in% common_ids) %>%
  column_to_rownames("sampleid")

# Make sure metadata rows are in the same order as feature table rows
metadata <- metadata[rownames(feature_table), , drop = FALSE]

# Order the metadata age_d
metadata <- metadata %>%
  mutate(
    age_d = factor(age_d, levels = c("day 1", "day 5", "day 35", "day 63"), ordered = FALSE)
  )
```

- stop here if you want to do ASV-level analysis and assign taxonomy later

Do this step if you want to collapse table to genus level before running maaslin3 
"This is to genus level or the lowest classified level"
```
feature_table_g <- feature_table %>%
  rownames_to_column("sampleid") %>%
  pivot_longer(-sampleid, names_to = "feature", values_to = "abundance")

taxonomy_collapsed <- taxonomy %>%
  transmute(
    feature = `feature`,
    collapsed_taxon = case_when(
      str_detect(Taxon, "g__[^;]+") ~ paste0("g_", str_remove(str_extract(Taxon, "g__[^;]+"), "^g__")),
      str_detect(Taxon, "f__[^;]+") ~ paste0("f_", str_remove(str_extract(Taxon, "f__[^;]+"), "^f__")),
      str_detect(Taxon, "o__[^;]+") ~ paste0("o_", str_remove(str_extract(Taxon, "o__[^;]+"), "^o__")),
      str_detect(Taxon, "c__[^;]+") ~ paste0("c_", str_remove(str_extract(Taxon, "c__[^;]+"), "^c__")),
      str_detect(Taxon, "p__[^;]+") ~ paste0("p_", str_remove(str_extract(Taxon, "p__[^;]+"), "^p__")),
      str_detect(Taxon, "d__[^;]+") ~ paste0("d_", str_remove(str_extract(Taxon, "d__[^;]+"), "^d__")),
      TRUE ~ "unclassified"
    )
  )

feature_table_genus <- feature_table_g %>%
  left_join(taxonomy_collapsed, by = "feature") %>%
  group_by(sampleid, collapsed_taxon) %>%
  summarise(abundance = sum(abundance), .groups = "drop") %>%
  pivot_wider(
    names_from = collapsed_taxon,
    values_from = abundance,
    values_fill = 0
  ) %>%
  column_to_rownames("sampleid")
```