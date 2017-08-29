# tag-co-occurrence
### creating tag co-occurrence network ###
### Aug28, 2017 ###

setwd("C:/Users/rwz7589/Desktop/Rfiles")
#files saved as text format
install.packages("dplyr")
install.packages("ggnetwork")
install.packages("readr")
install.packages("stringr")
install.packages("tnet")
install.packages("network")

library(dplyr)
library(ggnetwork)
library(ggplot2)
library(readr)
library(stringr)
library(tnet)
library(network)

#build a weighted edgelist

la <- read_tsv("latags.txt")$keywords %>% 
  str_split(",") %>%
  lapply(function(x) {
    expand.grid(x, x, w = 1 / length(x), stringsAsFactors = FALSE)
  }) %>%
  bind_rows

la <- apply(la[, -3], 1, str_sort) %>%
  t %>%
  data.frame(stringsAsFactors = FALSE) %>%
  mutate(w=la$w)
la

#drop selfloops
la<-group_by(la,X1,X2) %>%
filter(X1 != X2)

#save la file
write.csv(la,"la.csv")

#sf file

#build a weighted edgelist

sf <- read_tsv("sftags.txt")$keywords %>% 
  str_split(",| and ") %>%
  lapply(function(x) {
    expand.grid(x, x, w = 1 / length(x), stringsAsFactors = FALSE)
  }) %>%
  bind_rows

sf <- apply(sf[, -3], 1, str_sort) %>%
  t %>%
  data.frame(stringsAsFactors = FALSE) %>%
  mutate(w=sf$w)
sf

#drop selfloops
sf<-group_by(sf,X1,X2) %>%
  filter(X1 != X2)
sf

#save sf file
write.csv(sf,"sf.csv")
