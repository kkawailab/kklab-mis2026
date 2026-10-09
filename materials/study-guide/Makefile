# 経営情報 2026　学習ガイドのビルド
#   make          study-guide.pdf を作る（目次のページ番号を揃えるため2回組む）
#   make open     作った PDF を開く
#   make clean    中間ファイルを消す

LUALATEX := lualatex -interaction=nonstopmode -halt-on-error -file-line-error
BUILD    := build
SRCS     := study-guide.tex preamble.tex cover.tex intro.tex appendix.tex \
            $(wildcard sessions/*.tex) $(wildcard quiz/*.tex)

.PHONY: all open clean

all: study-guide.pdf

study-guide.pdf: $(SRCS) | $(BUILD)
	$(LUALATEX) -output-directory=$(BUILD) study-guide.tex >/dev/null
	$(LUALATEX) -output-directory=$(BUILD) study-guide.tex >/dev/null
	cp $(BUILD)/study-guide.pdf $@

$(BUILD):
	mkdir -p $(BUILD)

open: study-guide.pdf
	xdg-open $< >/dev/null 2>&1 &

clean:
	rm -rf $(BUILD)
