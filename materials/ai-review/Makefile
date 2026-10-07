# 経営情報 2026　AI利用ガイド　復習教材のビルド
#   make                 すべての版を作る（以下の全部）
#   make all-in-one      ワークシート全単元版（worksheets.pdf）
#   make single          ワークシート各単元（pdf/worksheet-NN.pdf）
#   make answers         記入例の全単元版（answers.pdf）
#   make answers-single  記入例の各単元（pdf/answers-NN.pdf）
#   make word            Word 版（word/ 以下。ワークシートと記入例の全単元版・各単元）
#   make 03              復習3のワークシートだけ（pdf/worksheet-03.pdf）
#   make answers-03      復習3の記入例だけ（pdf/answers-03.pdf）
#   make clean           中間ファイルを消す
#   make -j で並行ビルドできる。

LUALATEX := lualatex -interaction=nonstopmode -halt-on-error -file-line-error
BUILD    := build
NUMS     := 01 02 03 04 05
SINGLES  := $(NUMS:%=pdf/worksheet-%.pdf)
ANSWERS  := $(NUMS:%=pdf/answers-%.pdf)
DOCXS    := word/worksheets.docx $(NUMS:%=word/worksheet-%.docx) \
            word/answers.docx $(NUMS:%=word/answers-%.docx)
COMMON   := preamble.tex

.PHONY: all all-in-one single answers answers-single word clean $(NUMS) $(NUMS:%=answers-%)

all: all-in-one single answers answers-single word

all-in-one: worksheets.pdf
single: $(SINGLES)
answers: answers.pdf
answers-single: $(ANSWERS)
word: $(DOCXS)

$(NUMS): %: pdf/worksheet-%.pdf
$(NUMS:%=answers-%): answers-%: pdf/answers-%.pdf

# 目次のページ番号を揃えるため2回組む
worksheets.pdf: worksheets.tex cover.tex $(COMMON) $(NUMS:%=sessions/%.tex) | $(BUILD)
	$(LUALATEX) -output-directory=$(BUILD) worksheets.tex >/dev/null
	$(LUALATEX) -output-directory=$(BUILD) worksheets.tex >/dev/null
	cp $(BUILD)/worksheets.pdf $@

answers.pdf: answers.tex answers-cover.tex answers-preamble.tex $(COMMON) $(NUMS:%=answers/%.tex) | $(BUILD)
	$(LUALATEX) -output-directory=$(BUILD) answers.tex >/dev/null
	$(LUALATEX) -output-directory=$(BUILD) answers.tex >/dev/null
	cp $(BUILD)/answers.pdf $@

pdf/worksheet-%.pdf: sessions/%.tex session.tex $(COMMON) | $(BUILD) pdf
	$(LUALATEX) -output-directory=$(BUILD) -jobname=worksheet-$* '\def\sessionno{$*}\input{session}' >/dev/null
	cp $(BUILD)/worksheet-$*.pdf $@

pdf/answers-%.pdf: answers/%.tex answers-session.tex answers-preamble.tex $(COMMON) | $(BUILD) pdf
	$(LUALATEX) -output-directory=$(BUILD) -jobname=answers-$* '\def\sessionno{$*}\input{answers-session}' >/dev/null
	cp $(BUILD)/answers-$*.pdf $@

# Word 版は sessions/NN.tex・answers/NN.tex を tex2docx.py で変換する（内容は PDF 版と同じ）
word/worksheets.docx: tex2docx.py cover.tex $(NUMS:%=sessions/%.tex) | word
	python3 tex2docx.py -o $@

word/worksheet-%.docx: sessions/%.tex tex2docx.py | word
	python3 tex2docx.py -o $@ $*

word/answers.docx: tex2docx.py answers-cover.tex $(NUMS:%=answers/%.tex) | word
	python3 tex2docx.py --answers -o $@

word/answers-%.docx: answers/%.tex tex2docx.py | word
	python3 tex2docx.py --answers -o $@ $*

$(BUILD) pdf word:
	mkdir -p $@

clean:
	rm -rf $(BUILD)
