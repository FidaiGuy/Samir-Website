---
title: 'Miniature Brush up Project using R'
author: 'Samir Fidai'
date: '2023-03-07'
slug: []
categories: []
tags: []
---

Great to be back using R!




```r
horror_movies <- read_csv("C:/Users/samir/Documents/DATA/horror_movies.csv")
```

```
## New names:
## Rows: 32540 Columns: 21
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (10): original_title, title, original_language, overview, tagline, post... dbl
## (9): ...1, id, popularity, vote_count, vote_average, budget, revenue, ... lgl
## (1): adult date (1): release_date
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```

```r
languages <- read.csv("C:/Users/samir/Documents/DATA/list_of_iso_codes-934j.csv")
languages <- languages %>%
  select(Language.name, X639.1) %>%
  rename(original_language = X639.1)
horror_movies %>% glimpse()
```

```
## Rows: 32,540
## Columns: 21
## $ ...1              <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 1…
## $ id                <dbl> 760161, 760741, 882598, 756999, 772450, 1014226, 717…
## $ original_title    <chr> "Orphan: First Kill", "Beast", "Smile", "The Black P…
## $ title             <chr> "Orphan: First Kill", "Beast", "Smile", "The Black P…
## $ original_language <chr> "en", "en", "en", "en", "es", "es", "en", "en", "en"…
## $ overview          <chr> "After escaping from an Estonian psychiatric facilit…
## $ tagline           <chr> "There's always been something wrong with Esther.", …
## $ release_date      <date> 2022-07-27, 2022-08-11, 2022-09-23, 2022-06-22, 202…
## $ poster_path       <chr> "/pHkKbIRoCe7zIFvqan9LFSaQAde.jpg", "/xIGr7UHsKf0URW…
## $ popularity        <dbl> 5088.584, 2172.338, 1863.628, 1071.398, 1020.995, 93…
## $ vote_count        <dbl> 902, 584, 114, 2736, 83, 1, 125, 1684, 73, 1035, 637…
## $ vote_average      <dbl> 6.9, 7.1, 6.8, 7.9, 7.0, 1.0, 5.8, 7.0, 6.5, 6.8, 7.…
## $ budget            <dbl> 0, 0, 17000000, 18800000, 0, 0, 20000000, 68000000, …
## $ revenue           <dbl> 9572765, 56000000, 45000000, 161000000, 0, 0, 289259…
## $ runtime           <dbl> 99, 93, 115, 103, 0, 0, 88, 130, 90, 106, 98, 89, 97…
## $ status            <chr> "Released", "Released", "Released", "Released", "Rel…
## $ adult             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FAL…
## $ backdrop_path     <chr> "/5GA3vV1aWWHTSDO5eno8V5zDo8r.jpg", "/2k9tBql5GYH328…
## $ genre_names       <chr> "Horror, Thriller", "Adventure, Drama, Horror", "Hor…
## $ collection        <dbl> 760193, NA, NA, NA, NA, NA, 94899, NA, NA, 950289, N…
## $ collection_name   <chr> "Orphan Collection", NA, NA, NA, NA, NA, "Jeepers Cr…
```


```r
horror_movies_runtime <- horror_movies %>%
  mutate(original_language = str_replace_all(original_language, 'cn', 'zh')) %>%
  filter(runtime != is.na(runtime)) %>%
  select(original_language, runtime) %>%
  group_by(original_language)%>% 
  summarise(median_runtime = median(runtime), number_movies = n()) %>%
  arrange(desc(number_movies)) %>%
  head(n=25)

horror_movies_runtime %>%
  left_join(languages, by = 'original_language') %>%
  ggplot(aes(reorder(Language.name, -median_runtime), median_runtime))+
          geom_col() +
          geom_text(aes(label = median_runtime, vjust = -0.5)) +
          xlab('Language') +
          ylab('Median Minutes of Movie')
```

<img src="{{< blogdown/postref >}}index_files/figure-html/pressure-1.png" width="1920" />



