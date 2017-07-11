AUTOMAKE_OPTIONS = check-news
ACLOCAL_AMFLAGS = -I local_m4

MODEL_DIR := mni-models

# mess with the data install directory
pkgdatadir = $(prefix)/share/$(MODEL_DIR)
tagdir = $(prefix)/share/classify-Colin

MODEL_BASE := colin27_t1_tal_lin
MODELS := $(MODEL_BASE)

# datestamp
DATE_STAMP := `date +%D-%T`

# package data
noinst_DATA := \
	$(MODELS:=.mnc.gz) \
	$(MODEL_BASE)_mask.mnc.gz \
	$(MODEL_BASE)_mask.obj.gz \
	$(MODEL_BASE)_headmask.mnc.gz \
	$(MODEL_BASE)_vent.mnc.gz \
	$(MODEL_BASE)_vent.obj.gz \
	$(MODEL_BASE)_ntags_1000_bg.tag.gz \
	$(MODEL_BASE)_ntags_1000_nobg.tag.gz \
	Cerebellum_Ventricles_SubCortical_Mask-colin.mnc.gz

tag_DATA := \
	$(MODEL_BASE)_ntags_1000_bg.tag \
	$(MODEL_BASE)_ntags_1000_nobg.tag

# installed data
pkgdata_DATA := \
	$(MODELS:=.mnc) \
	$(MODELS:=_mask.mnc) \
	$(MODELS:=_mask.obj) \
	$(MODELS:=_headmask.mnc) \
	$(MODELS:=_vent.mnc) \
	$(MODELS:=_vent.obj) \
	Cerebellum_Ventricles_SubCortical_Mask-colin.mnc

EXTRA_DIST = $(noinst_DATA) $(m4_files) \
	epm-header.in

m4_files = local_m4/ac_prog_gzip.m4

# be tidy
CLEANFILES = $(pkgdata_DATA) $(tag_DATA)

# gunzip rule
%: %.gz
	$(gzip) -d -c $< > $@

all: $(noinst_DATA:.gz=) $(DATE)
	for i in $(pkgdata_DATA); \
	do \
	  if [ $${i: -4} == ".mnc" ]; then \
	  echo "Adding mni-model header to $$i"; \
	  minc_modify_header \
			-sinsert mni-model:package=$(PACKAGE) \
			-sinsert mni-model:version=$(VERSION) \
			-sinsert mni-model:compile-date=$(DATE_STAMP) \
			-sinsert mni-model:compile-user=$$USER \
			$$i; \
	  fi \
	done

