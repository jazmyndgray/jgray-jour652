
<!-- rnb-text-begin -->

---
title: "R Notebook"
output: html_notebook
---

This is an [R Markdown](http://rmarkdown.rstudio.com) Notebook. When you execute code within the notebook, the results appear beneath the code. 

Try executing this chunk by clicking the *Run* button within the chunk or by placing your cursor inside it and pressing *Cmd+Shift+Enter*. 


<!-- rnb-text-end -->



<!-- rnb-text-begin -->




<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-output-begin eyJkYXRhIjoiXG48IS0tIHJuYi1zb3VyY2UtYmVnaW4gZXlKa1lYUmhJam9pWUdCZ2NseHViR2xpY21GeWVTaDBhV1I1ZG1WeWMyVXBYRzVzYVdKeVlYSjVLSE5tS1Z4dWJHbGljbUZ5ZVNocVlXNXBkRzl5S1Z4dWJHbGljbUZ5ZVNoc2RXSnlhV1JoZEdVcFhHNWdZR0FpZlE9PSAtLT5cblxuYGBgclxubGlicmFyeSh0aWR5dmVyc2UpXG5saWJyYXJ5KHNmKVxubGlicmFyeShqYW5pdG9yKVxubGlicmFyeShsdWJyaWRhdGUpXG5gYGBcblxuPCEtLSBybmItc291cmNlLWVuZCAtLT5cbiJ9 -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeSh0aWR5dmVyc2UpXG5saWJyYXJ5KHNmKVxubGlicmFyeShqYW5pdG9yKVxubGlicmFyeShsdWJyaWRhdGUpXG5gYGAifQ== -->

```r
library(tidyverse)
library(sf)
library(janitor)
library(lubridate)
```

<!-- rnb-source-end -->


<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


Add a new chunk by clicking the *Insert Chunk* button on the toolbar or by pressing *Cmd+Option+I*.


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-output-begin eyJkYXRhIjoiXG48IS0tIHJuYi1zb3VyY2UtYmVnaW4gZXlKa1lYUmhJam9pWUdCZ2NseHVhR2x6ZEc5eWFXTmhiRjloYVhKZmNHOXNiSFYwYVc5dVgyeGhhRzl5WlNBOExTQnlaV0ZrTG1OemRpaGNJbjR2UkdWemEzUnZjQzlLVDFWU0wwZHBkRWgxWWk5cVozSmhlUzFxYjNWeU5qVXlMMlpwYm1Gc0xYQnliMnBsWTNRdlpHRjBZUzEyYVhvdmFHbHpkRzl5YVdOaGJGOWhhWEpmY0c5c2JIVjBhVzl1WDJGc2JGOXNZV2h2Y21VdVkzTjJYQ0lwWEc1Y2JtaHBjM1J2Y21sallXeGZZV2x5WDNCdmJHeDFkR2x2Ymw5c1lXaHZjbVZmWTJ4bFlXNGdQQzBnYUdsemRHOXlhV05oYkY5aGFYSmZjRzlzYkhWMGFXOXVYMnhoYUc5eVpTQjhQaUJqYkdWaGJsOXVZVzFsY3lncElIdytYRzRnSUcxMWRHRjBaU2gwYVcxbFBXaHZkWElvZEdsdFpYTjBZVzF3S1NrZ2ZENGdiWFYwWVhSbEtHUmhkR1VnUFNCa1lYUmxLSFJwYldWemRHRnRjQ2twSUh3K0lHMTFkR0YwWlNoNVpXRnlJRDBnZVdWaGNpaDBhVzFsYzNSaGJYQXBLVnh1WEc1M2NtbDBaVjlqYzNZb2FHbHpkRzl5YVdOaGJGOWhhWEpmY0c5c2JIVjBhVzl1WDJ4aGFHOXlaVjlqYkdWaGJpd2dYQ0orTDBSbGMydDBiM0F2U2s5VlVpOUhhWFJJZFdJdmFtZHlZWGt0YW05MWNqWTFNaTltYVc1aGJDMXdjbTlxWldOMEwyUmhkR0V0ZG1sNkwyaHBjM1J2Y21sallXeGZZV2x5WDNCdmJHeDFkR2x2Ymw5c1lXaHZjbVZmWTJ4bFlXNHVZM04yWENJcFhHNWdZR0FpZlE9PSAtLT5cblxuYGBgclxuaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZSA8LSByZWFkLmNzdihcIn4vRGVza3RvcC9KT1VSL0dpdEh1Yi9qZ3JheS1qb3VyNjUyL2ZpbmFsLXByb2plY3QvZGF0YS12aXovaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2FsbF9sYWhvcmUuY3N2XCIpXG5cbmhpc3RvcmljYWxfYWlyX3BvbGx1dGlvbl9sYWhvcmVfY2xlYW4gPC0gaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZSB8PiBjbGVhbl9uYW1lcygpIHw+XG4gIG11dGF0ZSh0aW1lPWhvdXIodGltZXN0YW1wKSkgfD4gbXV0YXRlKGRhdGUgPSBkYXRlKHRpbWVzdGFtcCkpIHw+IG11dGF0ZSh5ZWFyID0geWVhcih0aW1lc3RhbXApKVxuXG53cml0ZV9jc3YoaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZV9jbGVhbiwgXCJ+L0Rlc2t0b3AvSk9VUi9HaXRIdWIvamdyYXktam91cjY1Mi9maW5hbC1wcm9qZWN0L2RhdGEtdml6L2hpc3RvcmljYWxfYWlyX3BvbGx1dGlvbl9sYWhvcmVfY2xlYW4uY3N2XCIpXG5gYGBcblxuPCEtLSBybmItc291cmNlLWVuZCAtLT5cbiJ9 -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZSA8LSByZWFkLmNzdihcIn4vRGVza3RvcC9KT1VSL0dpdEh1Yi9qZ3JheS1qb3VyNjUyL2ZpbmFsLXByb2plY3QvZGF0YS12aXovaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2FsbF9sYWhvcmUuY3N2XCIpXG5cbmhpc3RvcmljYWxfYWlyX3BvbGx1dGlvbl9sYWhvcmVfY2xlYW4gPC0gaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZSB8PiBjbGVhbl9uYW1lcygpIHw+XG4gIG11dGF0ZSh0aW1lPWhvdXIodGltZXN0YW1wKSkgfD4gbXV0YXRlKGRhdGUgPSBkYXRlKHRpbWVzdGFtcCkpIHw+IG11dGF0ZSh5ZWFyID0geWVhcih0aW1lc3RhbXApKVxuXG53cml0ZV9jc3YoaGlzdG9yaWNhbF9haXJfcG9sbHV0aW9uX2xhaG9yZV9jbGVhbiwgXCJ+L0Rlc2t0b3AvSk9VUi9HaXRIdWIvamdyYXktam91cjY1Mi9maW5hbC1wcm9qZWN0L2RhdGEtdml6L2hpc3RvcmljYWxfYWlyX3BvbGx1dGlvbl9sYWhvcmVfY2xlYW4uY3N2XCIpXG5gYGAifQ== -->

```r
historical_air_pollution_lahore <- read.csv("~/Desktop/JOUR/GitHub/jgray-jour652/final-project/data-viz/historical_air_pollution_all_lahore.csv")

historical_air_pollution_lahore_clean <- historical_air_pollution_lahore |> clean_names() |>
  mutate(time=hour(timestamp)) |> mutate(date = date(timestamp)) |> mutate(year = year(timestamp))

write_csv(historical_air_pollution_lahore_clean, "~/Desktop/JOUR/GitHub/jgray-jour652/final-project/data-viz/historical_air_pollution_lahore_clean.csv")
```

<!-- rnb-source-end -->


<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

 


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-output-begin eyJkYXRhIjoiXG48IS0tIHJuYi1zb3VyY2UtYmVnaW4gZXlKa1lYUmhJam9pWUdCZ2NseHVZbkpwWTJ0ZmEybHNibk1nUEMwZ2NtVmhaQzVqYzNZb1hDSitMMFJsYzJ0MGIzQXZTazlWVWk5SGFYUklkV0l2YW1keVlYa3RhbTkxY2pZMU1pOWtZWFJoTFhacGVpOUNjbWxqYTE5TGFXeHVjMTlKUjFCZlVFc3RUV0ZwYmk1amMzWmNJaWtnZkQ0Z1kyeGxZVzVmYm1GdFpYTW9LVnh1WEc1aWNtbGphMTlyYVd4dWMxOWxiV2x6YzJsdmJuTWdQQzBnY21WaFpDNWpjM1lvWENKK0wwUmxjMnQwYjNBdlNrOVZVaTlIYVhSSWRXSXZhbWR5WVhrdGFtOTFjalkxTWk5a1lYUmhMWFpwZWk5Q2NtbGphMTlMYVd4dWMxOUpSMUJmVUV0ZlpXMXBjM05wYjI1ZlpYTjBhVzFoZEdWekxtTnpkbHdpS1NCOFBpQmpiR1ZoYmw5dVlXMWxjeWdwWEc1Z1lHQWlmUT09IC0tPlxuXG5gYGByXG5icmlja19raWxucyA8LSByZWFkLmNzdihcIn4vRGVza3RvcC9KT1VSL0dpdEh1Yi9qZ3JheS1qb3VyNjUyL2RhdGEtdml6L0JyaWNrX0tpbG5zX0lHUF9QSy1NYWluLmNzdlwiKSB8PiBjbGVhbl9uYW1lcygpXG5cbmJyaWNrX2tpbG5zX2VtaXNzaW9ucyA8LSByZWFkLmNzdihcIn4vRGVza3RvcC9KT1VSL0dpdEh1Yi9qZ3JheS1qb3VyNjUyL2RhdGEtdml6L0JyaWNrX0tpbG5zX0lHUF9QS19lbWlzc2lvbl9lc3RpbWF0ZXMuY3N2XCIpIHw+IGNsZWFuX25hbWVzKClcbmBgYFxuXG48IS0tIHJuYi1zb3VyY2UtZW5kIC0tPlxuIn0= -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYnJpY2tfa2lsbnMgPC0gcmVhZC5jc3YoXCJ+L0Rlc2t0b3AvSk9VUi9HaXRIdWIvamdyYXktam91cjY1Mi9kYXRhLXZpei9Ccmlja19LaWxuc19JR1BfUEstTWFpbi5jc3ZcIikgfD4gY2xlYW5fbmFtZXMoKVxuXG5icmlja19raWxuc19lbWlzc2lvbnMgPC0gcmVhZC5jc3YoXCJ+L0Rlc2t0b3AvSk9VUi9HaXRIdWIvamdyYXktam91cjY1Mi9kYXRhLXZpei9Ccmlja19LaWxuc19JR1BfUEtfZW1pc3Npb25fZXN0aW1hdGVzLmNzdlwiKSB8PiBjbGVhbl9uYW1lcygpXG5gYGAifQ== -->

```r
brick_kilns <- read.csv("~/Desktop/JOUR/GitHub/jgray-jour652/data-viz/Brick_Kilns_IGP_PK-Main.csv") |> clean_names()

brick_kilns_emissions <- read.csv("~/Desktop/JOUR/GitHub/jgray-jour652/data-viz/Brick_Kilns_IGP_PK_emission_estimates.csv") |> clean_names()
```

<!-- rnb-source-end -->


<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



When you save the notebook, an HTML file containing the code and output will be saved alongside it (click the *Preview* button or press *Cmd+Shift+K* to preview the HTML file). 

The preview shows you a rendered HTML copy of the contents of the editor. Consequently, unlike *Knit*, *Preview* does not run any R code chunks. Instead, the output of the chunk when it was last run in the editor is displayed.


<!-- rnb-text-end -->

