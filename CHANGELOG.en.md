# MerQuR+: Changelog

## 3.9.0 (2026-11-22)

> "Societies that wish to live in comfort without working, without tiring and without producing are doomed to lose first their dignity, then their freedom, and then their independence and their future."
> — Mustafa Kemal Atatürk

MerQuR+ is an analysis environment that works with a license key, built on the MerQur 2.9.0 base. This release moves the base from 2.7.0 to 2.9.0; the number of templates becomes 226. **Results of the templates of the previous version do not change.**

#### Base version
- MerQur 2.9.0: 15 new analyses in finance and economics (causality, additional unit root tests, cointegration with breaks, forecast comparison, spillover and GFEVD, additional panel tests, regression with robust standard errors, Oaxaca-Blinder, trend and cycle filters, inequality and poverty, event study, Fama-MacBeth, portfolio optimization, investment performance, value at risk and expected shortfall).
- MerQur 2.9.0: 10 new analyses in clinical and epidemiological reporting (Table 1, trend in proportions, additive interaction, attributable fraction, landmark survival, RMST difference, Peto meta-analysis, cumulative meta-analysis, calibration and decision curve, minimization); 170 analyses in 21 categories.
- MacKinnon (1996) option for the Maddala-Wu (Fisher-ADF) p-value in additional panel tests (default MacKinnon 1994). The coefficients are used with the author's written permission.
- Confidence level selection, background calculations and the progress window are used in the new analyses.

#### New templates
- **Conditional extreme value VaR/ES** (`risk_var_es`): GARCH and GARCH-POT based value at risk and expected shortfall, Kupiec, Christoffersen and Basel backtests, McNeil-Frey and Acerbi-Szekely ES tests.
- **Fuzzy linear and nonlinear regression** (`bulanik_regresyon`): Tanaka, Diamond, FLAD and Kim-Bishu methods side by side.
- **Threshold regression and multivariate threshold VAR** (`esik_regresyon_tvar`): Hansen (2000) and Tsay (1998).
- **Additional panel tests** (`panel_ek_testler`): fifteen tests, each with a check box.
- **Unit root family** (`birim_kok_ailesi`): Phillips-Perron, KPSS bands, Fourier ADF and Fourier KPSS.
- **Cointegration** (`esbutunlesme_kirilmali`): Engle-Granger, Johansen, VECM impulse response and variance decomposition.
- **Fourier Toda-Yamamoto causality** (`nedensellik_ailesi`).
- **226 templates** (196 R, 30 Python), 25 categories (new: Finance), 3,340 adjustable parameters.

#### Definition decisions
Where sources differ on a definition, the default is the definition in the relevant paper or textbook; where the source does not decide, the choice is left to the user and stated in the template description. Details are in `CHANGELOG-3.9.0-ayrinti.md` (Turkish).

#### Fix
- The description of the Zivot-Andrews trimming fraction was rewritten to match the definition; the calculation did not change.

## 3.8.0 (2026-11-20)

> "Societies that wish to live in comfort without working, without tiring and without producing are doomed to lose first their dignity, then their freedom, and then their independence and their future."
> — Mustafa Kemal Atatürk

MerQuR+ is an analysis environment that works with a license key, built on the MerQur 2.7.0 base. This release contains fixes from a line-by-line review of the template library, new form options and a new template. **Results of some templates may differ from the previous version**; every change is listed template by template in [CHANGELOG-3.8.0-ayrinti.md](CHANGELOG-3.8.0-ayrinti.md) (in Turkish).

#### New
- **NIST SP 800-22 randomness tests** template: 15 tests and second-level analysis (proportion of sequences passing, uniformity of p-values). The tests to run are chosen with check boxes; all by default.
- **219 templates** (190 R, 29 Python), 24 categories, 3,143 adjustable parameters.
- **The installer knows R:** it reports the installed R and its version; on request it downloads and installs the latest R, Rtools (to compile brms and blavaan models) and the R packages the templates use, then installs MerQuR+. Packages are installed in bulk and in parallel. This step can be skipped; for silent installs enable it with `/RKUR=1`.

#### Fixes and new options
- In most templates the level order, decimal separator (`;` ⇒ comma), seed field (0 or empty ⇒ session seed) and missing-data behavior follow common rules.
- The default of every new option is the previous behavior. Fixes that change results (for example structural topic model, latent transition, staggered difference-in-differences, test equating, constrained ordination, Markov regime, structural VAR) are marked on the relevant line of the detail file.
- The Data tab no longer freezes on large files (for example 1 million rows): the table shows at most the first 10,000 rows, while analyses use all the data.
- An f-string incompatible with Python 3.11 (PERMANOVA/Mantel template) and a missing study name in network meta-analysis were fixed.

## 3.7.0 (2026-11-16): first release

> "Societies that wish to live in comfort without working, without tiring and without producing are doomed to lose first their dignity, then their freedom, and then their independence and their future."
> — Mustafa Kemal Atatürk

MerQuR+ is an analysis environment that works with a license key, built on the MerQur 2.7.0 base. It contains all analyses and fixes of MerQur 2.7.0 and adds validated analysis templates and code execution with Python and R. The capital R at the end refers to the R code behind the templates.

#### Templates
- **218 validated templates in 24 categories** (190 R, 28 Python). Each template has a parameter form, sample data and sample settings. Templates were compared with published sources and, where an equivalent procedure exists, with SAS; the SAS code is shown as well. "Save as template…" adds personal templates; the template tree has a search box. Highlights:
  - econometrics: ARDL/NARDL, structural-break unit roots, second-generation panel tests, structural VAR, extended GARCH, stochastic frontier analysis, data envelopment analysis, discrete choice;
  - clinical: clinical prediction models, penalized Cox, RMST, crossover trials, bioequivalence, parametric (binormal) and ROC-GLM ROC, reference intervals by group;
  - forest tree breeding and quantitative genetics: provenance–progeny trials, heritability and genetic correlations, selection indices, QST–FST, seed orchard diversity, clonal and multi-site trials;
  - genetics: AMOVA with DNA sequences, Bayesian phylogenetics with MrBayes, DNA barcoding, sequence alignment;
  - variety registration (DUS and VCU): COYD, COYU, off-types, GAIA distance, combined Location × Year randomized blocks;
  - ecology: community-weighted means and environmental gradient models, PERMANOVA and PERMDISP, occupancy and capture–recapture models;
  - time series: multifractal DFA (MFDFA), fractal dimension and Hurst exponent estimators, joinpoint and interrupted time series;
  - other: proportion meta-analysis, RI-CLPM, diallel, mixture experiments, spatio-temporal kriging, multivariate control charts, structural topic model (STM), cluster sampling.
- **Parameter settings window.** All meaningful options of the method are set in the Variables, Model, Options, Advanced and Output tabs; **OK and run** runs the analysis with these settings.
- **Multilingual template output.** Text output, table column names and cells, code comments and SAS equivalents are shown in the interface language (Turkish, English, Spanish); the decimal separator follows the language.
- **MrBayes 3.2.9 is included** (GNU GPL v3, a separate program; its license and source code are in the installation folder).

#### Code tab
- Run code with embedded Python 3.13 and your installed R 4.5.x, or optionally your own Python interpreter. The active dataset is available as `df`. Tables, figures and text become cards in the Results, Table, Chart and Report tabs, go into the DOCX report with their code, and are saved with the `.mqr` project.
- Code runs in a separate worker process: Cancel stops it within a second, the time limit is configurable, and the main window survives a crashed worker. Syntax errors are highlighted on their line; missing packages come with an install suggestion; a run history is kept.
- **`mq` module:** call MerQur's analysis engines from code; `mq.veriye_yaz` writes new columns or filtered rows to the Data tab after a confirmation dialog, and the change can be undone.
- **`.mqs` code steps:** the GUI asks for approval for each code step; headless runs execute code steps only with `--kod-izin`.

#### Software and citations
- Every code and template run lists MerQuR+, R/Python and the packages used, with their versions and official citations (and MrBayes when it was run). The section stays in the report even when the code appendix is turned off. Re-running an `.mqs` script with different package versions shows a warning.
- Citation: Örücü, Ö. K. (2026). *MerQuR+: Integrated Academic Data Analysis and Reporting Platform* (Version 3.7.0; base version 2.7.0) [Computer software]. https://doi.org/10.53463/merqur.2026001

#### AI Interpreter
- Interpretation of results and reports with your own API key. Data cells are never sent; the AI does not compute or change results.

#### License, installation and updates
- Personal, one-year, offline-verified license key. The holder's name and institution appear in the About window and in the report footer. The MerQuR+ End User License Agreement applies.
- Runs **side by side** with MerQur on the same computer, with its own folder, settings, recent files list and icon. On first start, the language, theme, font size, chart DPI and AI provider settings of MerQur are copied once.
- The program makes no network requests on its own. **Help → Check for updates** runs only when you click it.
- Installer `MerQuRPlus-3.7.0-windows-x64-Setup.exe`; the installer and the program are code-signed by Ömer Kamil Örücü.

Contact: info@sekizgenacademy.com
