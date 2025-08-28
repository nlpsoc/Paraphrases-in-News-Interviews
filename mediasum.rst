MediaSum Corpus
===============

A collection of Interview transcripts from CNN / NPR.

Across the 10 seasons there are 463,596 conversations with ~49.4K
NPR transcripts and ~414.2K CNN transcripts. There is a total of 13,919,244 utterances, and 718,483 speakers.

Across 600 pairs of utterances, there are 5,581 annotations done by 112 annotators on whether the second utterance is a paraphrase of the first.

The original dataset is available `here <https://drive.google.com/file/d/1ZAKZM1cGhEw2A4_n4bGGMYyF8iPjLZni/view?usp=sharing>`_. It was originally distributed with `MediaSum: A Large-scale Media Interview Dataset for Dialogue Summarization <https://github.com/zcgzcgzcg1/MediaSum/>`_, Chenguang Zhu, Yang Liu, Jie Mei, Michael Zeng. Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, NAACL'21, 2021.

The original annotation of paraphrases is available `here <https://huggingface.co/datasets/AnnaWegmann/Paraphrases-in-Interviews>`_. It was originally distributed with `What's Mine becomes Yours: Detecting Context-Dependent Paraphrases in News Interview Dialogs <https://github.com/nlpsoc/Paraphrases-in-News-Interviews/tree/main>`_, Anna Wegmann, Tijs A. van den Broek, Dong Nguyen, Proceedings of the 2024 Conference on Empirical Methods in Natural Language Processing, EMNLP'24, 2024.

Dataset details
---------------

Speaker-level information
^^^^^^^^^^^^^^^^^^^^^^^^^

Speakers in this dataset are participants in an interview. This could be interview hosts or guests. The original dataset provides each speaker's name as a string, e.g. "ED LAVANDERA, CNN CORRESPONDENT". We index Speakers by these strings.

Note: In the speaker list, authors sometimes have non-unique identifiers (e.g., ‘ED LAVANDERA, CNN CORRESPONDENT’, ‘LAVANDERA’ or ‘E. LAVANDERA’ refer to the same speaker). Further, each identifier that is the same in one conversation as in another is considered the same speaker. This might be incorrect for cases like 'UNIDENTIFIED MALE' or 'UNIDENTIFIED FEMALE' that are sometimes used in interviews.


Utterance-level information
^^^^^^^^^^^^^^^^^^^^^^^^^^^

For each Utterance we provide:

- id: ``<str>``, the index of the utterance in the format `BROADCASTER-CONVONBR-UTTNBR`, where *BROADCASTER* is NPR or CNN, *CONVONBR* is the conversation number, *UTTNBR* is the utterance number, (e.g. *CNN-177596-7* or *NPR-4-1*).
- conversation_id: ``<str>``, conversation_id: id in the format `BROADCASTER-CONVONBR` (e.g. *CNN-177596* or *NPR-4*). This corresponds to the original ids in the MediaSum dataset.
- speaker: ``<Speaker>``, the speaker object who authored the utterance, name available via .speaker.id
- reply_to: ``<str>``, the id of the utterance to which this utterance replies to. `None` if the utterance is the first in a conversation.
- timestamp: ``None``. Our dataset does not contain timestamp information for utterances.
- text: ``<str>``, the textual content of the utterance.
- meta: ``<dict>``, a dictionary containing additional metadata about the utterance. See below for details.
- vectors: TODO??

The available metadata is about the paraphrase annotations in 2-person interviews. For 600 utterance pairs (u, v), there are annotations on whether v contains a paraphrase of u. Annotations contains paraphrase spans selected by Prolific annotators. v is always the interview host. The annotations include: character entities (or who is referred to in the utterance), emotion, a tokenized version of the text, caption information, and notes about the transcript, which we describe as follows:

- paraphrase_is_host: ``<bool>``, whether the utterance is spoken by the host of the interview. This also tells us whether the utterance is the second in a pair of utterances annotated for paraphrases.
- paraphrase_number_votes ``<int>``, the number of annotators who annotated this utterance pair.
- paraphrase_votes ``<int>``, the number of annotators who indicated that the second utterance contains a paraphrase of the first.
- paraphrase_ratio ``<float>``, the ratio of paraphrase votes to total votes.
- paraphrase_guest_highlights ``list <[int]>``, a list based on the tokens that can be created from utterance.text.split(). Each entry is between 0 and paraphrase_votes, indicating how many annotators highlighted that token as part of a paraphrase for the next utterance.
- paraphrase_host_highlights ``list <[int]>``, a list based on the tokens that can be created from utterance.text.split(). Each entry is between 0 and paraphrase_votes, indicating how many annotators highlighted that token as part of a paraphrase of the previous utterance.
- paraphrase_PROLIFIC_X ``list <int>``, for X in [1, 2, ..., 112], a list of length utterance.text.split() indicating whether annotator X highlighted token i as part of a paraphrase (1) or not (0). If annotator X did not annotate this key does not exist in the dict.

Example of utterance "CNN-177596-7":


>>> print(media_sum_corpus.get_utterance("CNN-177596-7"))
    Utterance(id: 'CNN-177596-7', conversation_id: CNN-177596, reply-to: CNN-177596-6,
        speaker: Speaker(id: 'JOHNS', vectors: [], meta: ConvoKitMeta({'name': 'JOHNS'})),
        timestamp: None, text: 'This is not good.', vectors: [],
        meta: ConvoKitMeta({
            'paraphrase_guest_highlights': [10, 9, 9, 9], 'paraphrase_is_host': False,
            'paraphrase_number_votes': 20, 'paraphrase_votes': 10, 'paraphrase_ratio': 0.5,
            'paraphrase_PROLIFIC_1': [0, 0, 0, 0], 'paraphrase_PROLIFIC_2': [1, 1, 1, 1], 'paraphrase_PROLIFIC_3': [0, 0, 0, 0], 'paraphrase_PROLIFIC_4': [0, 0, 0, 0], 'paraphrase_PROLIFIC_5': [0, 0, 0, 0], 'paraphrase_PROLIFIC_6': [1, 1, 1, 1], 'paraphrase_PROLIFIC_7': [1, 0, 0, 0], 'paraphrase_PROLIFIC_8': [1, 1, 1, 1], 'paraphrase_PROLIFIC_9': [0, 0, 0, 0], 'paraphrase_PROLIFIC_10': [0, 0, 0, 0], 'paraphrase_PROLIFIC_11': [1, 1, 1, 1], 'paraphrase_PROLIFIC_12': [0, 0, 0, 0], 'paraphrase_PROLIFIC_13': [1, 1, 1, 1], 'paraphrase_PROLIFIC_14': [0, 0, 0, 0], 'paraphrase_PROLIFIC_15': [0, 0, 0, 0], 'paraphrase_PROLIFIC_16': [1, 1, 1, 1], 'paraphrase_PROLIFIC_17': [0, 0, 0, 0], 'paraphrase_PROLIFIC_18': [1, 1, 1, 1], 'paraphrase_PROLIFIC_19': [1, 1, 1, 1], 'paraphrase_PROLIFIC_20': [1, 1, 1, 1]
        }))

Conversation-level information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Conversations represent scenes of the show. They are indexed by the id *sXX-eYY-cZZ*, where *XX* denotes the season (e.g. 01), *YY* denotes the episode (e.g. 01), *ZZ* denotes the conversation (e.g. 01).

- season: ``<str>``, the index of the season in the format *sXX*, where XX starts at 01 for season 1 and increments to 10 for season 10.
- episode: ``<str>``, the index of the episode in the format *eXX*, where XX starts at 01 for the first episode of the season and increments accordingly.
- scene: ``<str>``, the index of the scene in the episode in the format *cXX*, where *XX* starts at 01 for the first scene of the episode and increments accordingly. Note that scenes are, for our intents and purposes, conversations.

Usage
-----

To download directly with ConvoKit:

>>> from convokit import Corpus, download
>>> corpus = Corpus(filename=download("friends-corpus"))


For some quick stats:

>>> corpus.print_summary_stats()
Number of Speakers: 700
Number of Utterances: 67373
Number of Conversations: 3107


Additional note
---------------

Data License
^^^^^^^^^^^^

The original data for this Corpus was obtained from Jinho D. Choi and the Emory NLP team (https://github.com/emorynlp/character-mining). It is copyrighted 2015, Emory University, and licensed under `the Apache License, v.2.0 <https://github.com/emorynlp/character-mining/blob/master/LICENSE.txt>`_.

Contact
^^^^^^^

Please email any questions to Emily Tseng (et397@cornell.edu), Nianyi Wang (nw344@cornell.edu), and Katharine Sadowski (ks2373@cornell.edu).