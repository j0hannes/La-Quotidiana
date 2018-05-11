xml_files := $(wildcard *.xml)
tsv_files := $(patsubst %.xml,%.tsv,$(xml_files))

default: $(tsv_files) list_variants compile_results

%.tsv: %.xml
	cat $< | tr "\n" " " | sed -r "s@</P>@\n@g" | sed -r -e "s@^.*?xml:lang=\"@@" -e "s@\"> <TEXT> <P> @\t@" | head -n-1 > $@

list_variants:
	$(eval variants=$(shell cat *.tsv | awk '{print $$1}' | sort -u | sed -r "s/^rm-//"))
	$(eval txt_files=$(patsubst %,%.txt,$(variants)))

%.txt:
	cat *.tsv | awk -F $$'\t' '{if ($$1 ~ /$*/) {print $$2}}' | sed -r -e "s@■ \([a-z/]+\) @@" -e "s@^■ @@" | grep "^[A-ZÄÖÜÀÈÎÒ]" | awk 'length >= 200' | sort -u > $@

compile_results:
	$(MAKE) $(txt_files)
