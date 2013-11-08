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
dist_pkgdata_DATA := \
	$(MODELS:=.mnc.gz) \
	$(MODEL_BASE)_mask.mnc.gz \
	$(MODEL_BASE)_mask.obj.gz \
	$(MODEL_BASE)_headmask.mnc.gz \
	$(MODEL_BASE)_vent.mnc.gz \
	$(MODEL_BASE)_vent.obj.gz \
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
	Cerebellum_Ventricles_SubCortical_Mask-colin.mnc \
	$(MODELS:=_16_blur.mnc) \
	$(MODELS:=_16_mask.mnc) \
	$(MODELS:=_2_blur.mnc) \
	$(MODELS:=_2_dxyz.mnc) \
	$(MODELS:=_2_mask.mnc) \
	$(MODELS:=_4_blur.mnc) \
	$(MODELS:=_4_dxyz.mnc) \
	$(MODELS:=_4_mask.mnc) \
	$(MODELS:=_8_blur.mnc) \
	$(MODELS:=_8_dxyz.mnc) \
	$(MODELS:=_8_mask.mnc)

EXTRA_DIST = $(m4_files) \
	$(tag_DATA) \
	epm-header.in

m4_files = local_m4/ac_prog_gzip.m4 \
	local_m4/ac_prog_make_model.m4

# be tidy
CLEANFILES = $(pkgdata_DATA) $(MODELS:=.mnc)

# gunzip rule
%: %.gz
	$(gzip) -d -c $< > $@

all: $(dist_pkgdata_DATA:.gz=) $(DATE)
	for i in $(MODELS); \
	do \
	  $(make_model) $$i; \
	done
	for i in $(pkgdata_DATA); \
	do \
	  if [[ "$$i" =~ ".mnc" ]]; then \
	  echo "Adding mni-model header to $$i"; \
	  minc_modify_header \
			-sinsert mni-model:package=$(PACKAGE) \
			-sinsert mni-model:version=$(VERSION) \
			-sinsert mni-model:compile-date=$(DATE_STAMP) \
			-sinsert mni-model:compile-user=$$USER \
			$$i; \
	  fi \
	done
