TEXFILE = main.tex
NOTE_EN = note/note_en.tex
IMG     = registry.gitlab.com/islandoftex/images/texlive:TL2025-2025-03-30-full

DOCKER_RUN = docker run --rm \
	-u $(shell id -u):$(shell id -g) \
	-v $(shell git rev-parse --show-toplevel):/workspace \
	-w /workspace/src \
	$(IMG)

.PHONY: all main note-en clean

all: main clean

main:
	$(DOCKER_RUN) latexmk $(TEXFILE)

note-en:
	$(DOCKER_RUN) latexmk -pdf $(NOTE_EN)

src/%.pdf: src/%.tex
	docker run --rm \
		-u $(shell id -u):$(shell id -g) \
		-v $(shell git rev-parse --show-toplevel)/stat-ml:/workspace \
		-w /workspace/$(dir $<) \
		$(IMG) \
		latexmk -r /workspace/src/.latexmkrc -pdfdvi $(notdir $<)

clean:
	$(DOCKER_RUN) /bin/bash -c "find . -type f -name '*.tex' -print0 | xargs -0 latexmk -r /workspace/src/.latexmkrc -c"
	$(DOCKER_RUN) find . -name "*-SAVE-ERROR" -delete
