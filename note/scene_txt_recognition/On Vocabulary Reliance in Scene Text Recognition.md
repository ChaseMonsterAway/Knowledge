# On Vocabulary Reliance in Scene Text Recognition

- **Motivation**

  - SOAT methods perform well on images with words within vocabulary but generalize poorly to images with words outside vocabulary  (**vocabulary reliance**)

- **Recommendation**

  - in vocabulary case
    - CNTX + Attn
  - outside vocabulary case
    - Non-CNTX + Segmentation

- **Analysis**

  - **phenomenon**

    - vocabulary reliance is ubiquitous
    - attention-based decoders prove weak in generalizing ability and segmentation-based decoders perform well in utilizing visual features
    - context modeling is highly coupled with the prediction layers

  - **How to analysis?**

    - **core**: in vocabulary or not in vocabulary

    - preparing data

      - test data

        - Ω = { IC13 + IC15 + SVT + SVTP + CUTE80 } 的test set    (in vocabulary)
        - $Ω^c$ = complemet of Ω                                               (outside vocabulary)
        - IIIT-I =  IIIT in vocabulary (1354)
        - IIIT-O = IIIT outside vocabulary (1646) 
        - ![avatar](D:\myfile\knowledge\Knowledge\note\scene_txt_recognition\imgs\ovrstr_dataset.png)

      - training data

          **Synthesis Detatils**: ..............................

        

        - LexiconSynth (LS)
          - uniformly sampling from collected ground truth words
          - word (LS) == word (Ω)
          - about 7 million
        - RandomSynth (RS)
          - generate from characters in a random permutation
          - **The lengths of the pseudowords are of the same distribution with those in LS, but the distribution of character classes is uniform. ???**
          - no vocabulary help
          - about 7 million
        - MixedSynth (MS)
          - MS = LS ∪ RS
          - RS : LS = r : (1-r), r∈[0, 1]
            - r=0 -> LS
            - r=1 -> RS

    - module combinations

      - typical str module

        - transformation (TRAN)
        - feature extraction (FEAT)
        - context modeling (CNTX)    ——  vocabulary reliance
        - prediction (PRED)                   ——  vocabulary reliance

      - target: verify vocabulary reliance,  **control variate method**

        - No TRAN

        - fix FEAT, eg. ResNet50

        - CNTX

          - BiLSTM
          - PPM

        - PRED

          - CTC
          - Attention

          - Segmentation (Cross entropy)

          ![avatar](D:\myfile\knowledge\Knowledge\note\scene_txt_recognition\imgs\ovrstr_model_acc.png)

    - metrics

      - general accuracy

      - performance gap

        Gap (train)  = Acc(train ,  in_vocabulary)  -  Acc(train ,  outside_vocabulary)

      - observation ability (OA): how accurately an algorithm recognizes words without any vocabulary given in training data

      - vocabulary learning ability(VA) : evaluating the recognition acc on limited vocabularies

      - vocabulary generalization (VG) : evaluate the vocabulary generalization ability

        **vg = 1 - (Gap(LS) - Gap(RS))**

      - harmonic mean

      - hm = 3( 1/OA  + 1/VA + 1/CG)^-1

        ![avatar](D:\myfile\knowledge\Knowledge\note\scene_txt_recognition\imgs\ovrstr_metric.png)

        

        

        
        
        