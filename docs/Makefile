PYTHON := python3.7
VENV := .venv
LANG := ja
ORIGDIR := docs
LOCALESDIR := locales
POTDIR := $(LOCALESDIR)/pot
PODIR := $(LOCALESDIR)/$(LANG)
TRANSDIR := docs.$(LANG)
PUBLISHDIR := ../../../cocoatomo.github.io/poetry-ja

DOCS := $(wildcard $(ORIGDIR)/*.md)
POTS := $(subst $(ORIGDIR), $(POTDIR), $(patsubst %.md, %.pot, $(DOCS)))
TRANSDOCS := $(subst $(ORIGDIR), $(TRANSDIR), $(DOCS))
# $(warning $(TRANSDOCS))

.PHONY: venv pushorig catalog pullpo translate build-$(LANG) publish-$(LANG)

venv:
	$(PYTHON) -m venv --prompt=poetrydoc $(VENV)
	$(VENV)/bin/pip install mkdocs pygments mkdocs-material

pushorig: catalog
	zanata-cli --batch-mode push

catalog: $(POTS)

$(POTDIR)/%.pot: $(ORIGDIR)/%.md
	po4a-gettextize \
	--master-charset UTF-8 \
	--format text \
	--option markdown \
	--master $< \
	--po $@

pullpo:
	zanata-cli --batch-mode pull --locales ja

translate: $(TRANSDOCS)

$(TRANSDIR)/%.md: $(PODIR)/%.po $(ORIGDIR)/%.md
	po4a-translate \
	--master-charset UTF-8 \
	--localized-charset UTF-8 \
	--format text \
	--option markdown \
	--keep 0 \
	--master $(ORIGDIR)/$(*F).md \
	--po $< \
	--localized $@

build-$(LANG): venv
	PATH=$(VENV)/bin:$$PATH mkdocs build -f mkdocs.$(LANG).yml --theme material

publish-$(LANG):
	rm -rf $(PUBLISHDIR)/*
	cp -rp site/* $(PUBLISHDIR)/
