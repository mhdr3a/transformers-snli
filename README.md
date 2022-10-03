## Evaluate a Model using [SNLI Development Set](https://nlp.stanford.edu/projects/snli/)

Here is an example of evaluating a model (fine-tuned either on MNLI or SNLI) using SNLI development set [Bowman et al., 2015](https://nlp.stanford.edu/pubs/snli_paper.pdf).

This is an example of using run_snli.py in Google Colab:

```bash
!git clone https://github.com/mhdr3a/transformers-snli
!mv /content/transformers-snli/* /content/
!rm transformers-snli -r
!pip install -r requirements.txt

# Download SNLI
!wget https://nlp.stanford.edu/projects/snli/snli_1.0.zip
!mkdir /content/data/ && mkdir /content/data/glue
!unzip /content/snli_1.0.zip -d /content/data/glue/
!rm -r /content/data/glue/__MACOSX && rm snli_1.0.zip
!mv /content/data/glue/snli_1.0 /content/data/glue/SNLI

!python run_snli.py \
        --task_name snli \
        --do_eval \
        --data_dir data/glue/SNLI \
        --model_name_or_path mnli-6 \
        --max_seq_length 128 \
        --output_dir mnli-6
```
* Note that the instructions above apply to models fine-tuned on MNLI; however, a model which is fine-tuned on SNLI could also be evaluated using these instructions with a few minor changes explained at the end of this document; nevertheless, I suggest
using [this repository](https://github.com/mhdr3a/cartography) to do so.

This will create the snli_predictions.txt file in ./mnli-6, which can then be evaluated using evaluate_predictions.py.

```bash
!python evaluate_predictions.py ./mnli-6/snli_predictions.txt
```

The evaluation results for the [mnli-6 model](https://huggingface.co/mahdiyar/mnli-6) is as follows:

```bash
entailment: 0.9035746470411535
neutral: 0.7669242658423493
contradiction: 0.8398413666870043

Overall SNLI Dev Evaluation Accuracy: 0.8374314163787848
```

* Refer to utils_snli.py (lines 131, 196, and 271), and apply the mentioned changes if you want to evaluate a model that is fine-tuned on the SNLI training set using above instructions. 
