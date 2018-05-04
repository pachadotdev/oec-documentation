# Data Processing

## Tidy Data

I followed Tidy Data principles. Those principles are closely tied to those of relational databases and Codd’s relational algebra. I only followed the principles exposed in [@tidydata2014] and [@r4ds2016] above all matters related to performing code and coding style.

<div class="figure">
<img src="fig/tidy-data.svg" alt="Data pipeline"  />
<p class="caption">(\#fig:unnamed-chunk-1)Data pipeline</p>
</div>

## Filling gaps in our data

I use mirrored flows to cover gaps in raw data. Some countries report zero exports for some products, but I can inspect what their trade partners reported. If country A reported zero exports (imports) of product B to (from) country C, then I searched what country C reported of imports (exports) of product B from (to) country A.

This approach has one major drawback and is that exports are reported FOB (free on board) while imports are reported CIF (cost, insurance and freight). There are different approaches to solve this difficulty, and in particular [@tradecosts2004], [@geography2009] and [@baci2010] discuss this in detail and propose that an 8% CIF/FOB ratio is suitable to discount costs and compare imports and exports.

Let $x_{c,c',p}$ represent the exports of country $c$ to country $c'$ in product $p$ and $m_{c',c,p}$ the imports of country $c'$ from country $c$. Under this notation I defined corrected flows as:

$$\hat{x}_{c,c',p} = \max\left\{x_{c,c',p}, \frac{m_{c',c,p}}{1.08}\right\}$$
$$\hat{m}_{c,c',p} = \max\left\{x_{c',c,p}, \frac{m_{c,c',p}}{1.08}\right\}$$

After symmetrization all observations are rounded to zero decimals.

<!--
## Classification conversion in monthly data

As [UN COMTRADE](https://comtrade.un.org/data/) states: "Monthly datasets may mix codes from multiple HS revisions and are provided as is except for standardization of trade flow and partner information, as well as conversion to US dollars."

Our solution to obtain tidy datasets was to convert all HS codes available in raw datasets to HS rev 2007. In \@ref(materials-of-interest) I provide links to correspondence tables that allow us to do that conversion process.
-->

## Countries not included in rankings and indicators

The curated data includes all the countries available from UN Comtrade data. However, RCA based calculations such as ECI, PCI, Proximity and Density explained in Chapter \@ref(the-mathematics-of-economic-complexity) consider 128 countries that account for 99% of world trade, 97% of the world’s total GDP and 95% of the world’s population according to [@atlas2014].

I considered simultaneously:

* Countries with population greater or equal to 1.2 million
* Countries whose traded value is greater or equal than 1 billion

<div class="figure">
<img src="fig/countries.svg" alt="Schematic of the procedure used to determine the countries that were included in the Atlas"  />
<p class="caption">(\#fig:unnamed-chunk-2)Schematic of the procedure used to determine the countries that were included in the Atlas</p>
</div>

## GitHub repositories

### Getting and cleaning data from UN COMTRADE

* [OEC Yearly Data](https://github.com/pachamaltese/oec-yearly-datasets)
* [OEC Monthly Data](https://github.com/pachamaltese/oec-monthly-datasets)

### Scraping data in The Atlas of Economic Complexity

* [OEC Atlas Data](https://github.com/pachamaltese/oec-atlas-data)

### Product space layouts

* [OEC Product Space](https://github.com/pachamaltese/oec-product-space)

### Product and country codes

* [OEC Observatory Codes](https://github.com/pachamaltese/oec-observatory-codes)
* [OEC Comtrade Codes](https://github.com/pachamaltese/oec-comtrade-codes)

### R packages (for reproducibility)

* [Packrat snapshot](https://github.com/pachamaltese/oec-packages-snapshot/)
* [Packrat bundles](https://github.com/pachamaltese/oec-bundles)

## Software versions

At the moment I am using R 3.4.3 and RStudio Server Pro 1.1 on Ubuntu Server 16.04.

I built R from binaries in order to obtain a setup linked with multi-threaded BLAS/LAPACK libraries. This build is linked to Intel MKL 2017 but my output can be reproduced if your R setup is linked to OpenBLAS, only performance differences should be noticed for some hardware.

## Hardware information

Our server features Intel© Xeon 2.27GHz (eight cores) processor and 32 GB (four DDR3 cards of eight gigabytes each).

The functions are executed using parallelization on four cores because empirically I detected and overhead due to data communication with the cores when using more cores.

Please notice that running our scripts with parallelization demands more RAM than the amount you can find on an average laptop. 

## Reproducibility notes

To guarantee reproducibility I provided [Packrat](https://rstudio.github.io/packrat/) snapshot and bundles. This prevents changes in syntax, functions or dependencies.

My R installation is isolated from apt-get to avoid any accidental updates that can alter the data pipeline and/or the output.

The projects are related to each other. In order to avoid multiple copies of files some projects read files from other projects. For example, [OEC Yearly Indicators](https://github.com/pachamaltese/oec-yearly-indicators) input is the output of [OEC Yearly Datasets](https://github.com/pachamaltese/oec-yearly-datasets) and [OEC Atlas Data](https://github.com/pachamaltese/oec-atlas-data).

The only reproducibility flaw of this project lies in data downloading. Obtaining raw datasets from UN COMTRADE demands an API key that can only be obtained from institutional access.

This project will not work on Windows without editing parts of the code. Multicore functionality supports multiple workers only on those operating systems that support the fork system call, and this excludes Windows.

## Coding style and performant code

I used [Tidyverse Style Guide](http://style.tidyverse.org/). As cornerstone references for performant code I followed [@advancedr2014] and [@masteringsoftware2017].

Some matrix operations are written in Rcpp to take advantage of C++ speed. To take full advantage of hardware and numerical libraries I am using sparse matrices as it is explained in [@rcpparmadillo2018].

## Materials of interest

* [Rankings from the Atlas (scraped from the original PDF)](https://github.com/pachamaltese/oec-atlas-data/tree/master/2-scraped-tables)
* [Country names with equivalency between UN numeric code, ISO 2 and ISO 3 (UN COMTRADE)](https://github.com/pachamaltese/oec-comtrade-codes/blob/master/country-codes.xls?raw=true)
* [UN COMTRADE product classifications](https://github.com/pachamaltese/oec-comtrade-codes/blob/master/official-list-of-comtrade-codes.xlsx?raw=true)
* [Layouts to visualize the product space](https://github.com/pachamaltese/oec-product-space/tree/master/hs92-sitc/2-layouts-d3plus1)
* [Layouts to visualize the product space (for D3plus 2)](https://github.com/pachamaltese/oec-product-space/tree/master/hs92-sitc/3-layouts-d3plus2)

UN COMTRADE product classifications may be of interest as it includes all trade classifications levels with detailed description of: (i) Harmonized System revisions 1992 (H0/HS92), 1996 (H1), 2002 (H2), 2007 (H3), 2012 (H4), 2017 (H5); Standard International Trade classification revisions 1 (S1), 2 (S2), 3 (S3), (iii) and 4 (S4); and Broad Economic Categories (BE).
