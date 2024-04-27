# Theory of Mind Performance of Large Language Models: A Comparative Analysis of Turkish and English

## Introduction

This is the code repository for the article with the title stated above, written by Burcu Ünlütabak and Onur Bal.

The authors can be contacted either at [burcuunlutabak@gmail.com](mailto:burcuunlutabak@gmail.com) or [onurbal101@gmail.com](mailto:onurbal101@gmail.com).

## Usage Guide

Here, three approaches of using this repository will be shared.

1) The "reader" path, where you can access the input files used to conduct the studies (including the system messages, stories, and questions), and the output files, which are the raw results on which our findings are based.
2) The "replicator" path, where one can simply follow the instructions and replicate the article's methodology, as is. This path requires an advanced-level knowledge of computer usage and entry-level programming knowledge. A text editor that is capable of running Jupyter Notebook files, such as Visual Studio Code, and a recent version of Python are needed. Please keep in mind that the results one gets by following this path may differ from ours. This is because of the non-deterministic nature inherent in large language models. Albeit a temperature of 0 was used, which makes the responses more deterministic, individual responses one receives may vary, though the statistical results remain the same when averaged out.
3) The last path, "explorer", in contrast, will enable one to alter the various settings which were used in the article, and add or remove new stories, system messages, question sets and prompts.

### Reader

* For the stories, questions, system messages and prompt groups used in the study, access `/input/parameters/01_parameters.xlsx` (detailed information regarding this spreadsheet file can be found in the "Parameters Spreadsheet" section)
* For the responses received by ChatGPT
  * if you're looking for the original 10 trials which were manually coded, access `/output/all_responses_coded.csv`
  * if you're looking for the latest 10 trials with log probability calculations (which was not available at the onset of this study), access `output/responses_combined/07_responses_combined.csv`
* For the coded responses, access `/input/coding/answers_ob.csv` and `/input/coding/answers_ob.csv` (only unique responses were coded since the same response to the same story and question pair does not need to be assessed more than once)
* For the statistical tables based on the coded responses, access the Excel files with several sheets each in the "output/tables" directory, specifically
  * `studies_analyses.xlsx` for running statistical analyses on the data
  * `studies_frequencies.xlsx` for the frequency distribution of the responses grouped by story and question
  * `studies_tom_types.xlsx` for the frequency distribution of the responses grouped by tom types

### Replicator

1) Download the repository and keep the directory structure intact
2) Run `notebooks/01_process_input.ipynb`, which using `input/parameters/01_parameters.xlsx` will create `input/03_parameters_expanded.csv`, used by the notebook in the next step for determining the scenarios and the questions to be asked
3) Run `notebooks/04_get_responses.ipynb`, which will create .csv files containing the responses in `output/responses_raw/`. You can run as many trials as you wish
4) Run `notebooks/06_combine_responses.ipynb`, which will create `output/responses_combined/07_combined_responses.csv`, a combined version of all the responses
5) Run `notebooks/13_prepare_for_analysis.ipynb`, which will modify `output/responses_combined/07_combined_responses.csv` with new properties used during analysis
6) Finally, run `notebooks/15_analysis.ipynb` to run the analyses

### Explorer

1) Download the repository and keep the directory structure intact.
2) Modify the `./input/parameters/01_parameters.xlsx` file as you see fit. One can add or remove *rows* from each sheet (except the headers) without any limitations. One should not (1) delete or rename sheets, (2) delete or rename columns.
3) Follow the steps of the "Replicator" path. Having changed the parameters file, the code will dynamically use the properties found there
   
## Parameters Spreadsheet

The `parameters.xlsx` spreadsheet is where all the information regarding our scenarios are to be found. We created this file, in the form it is, in order to

1) store all aspects of our study in a unified manner
2) seperate the "doing" (notebook scripts) from the "storing" (this file)

We want to underline the second reason, since we observed that in the literature the scenarios, the questions, and the system messages were all defined in the scripts themselves, which made them quite hard to parse. The parts associated with programming were not easily seperable from the parts associated with the design of the study. Hence, we wanted to build a script that was easy to read even for those not well-versed in programming. We also wanted our approach to be quite easy to adapt to different studies. By changing parameters such as the stories to be given, the questions to be asked, the system messages to be used in a simple spreadsheet file, one can run different studies without any alterations to the scripts themselves.

### The Sheets

There are 7 sheets in total, sorted in a top-to-bottom order. These are:

* `tbl_studies`
* `tbl_scenarios`
* `tbl_stories`
* `tbl_chats`
* `tbl_questions`
* `tbl_answers`
* `tbl_system_messages`

Let us analyse them one by one.

#### tbl_studies

This is where the different parts of our research are defined. In our project, we had three studies:

1) `Ullman Replication`
2) `Ullman Expansion`
3) `Second Order`

##### Columns

1) `study_id`: the unique integer ID of the study
2) `study_name`: the name of the study

#### tbl_system_messages

The system messages to be used with the different studies, defined in `tbl_studies`.

##### Columns

1) `system_message_id`: the unique integer ID of the system message
2) `study_id`: the ID of the study (found in `tbl_studies`) with which this system message is associated
3) `system_message_language`: the language of the system message (English or Turkish)
4) `system_message_content`: the content of the system message

#### tbl_scenarios

Here, one can find the `scenario`s to be sent to ChatGPT. A `scenario` can be defined as:

1) a system message (associated with the `study_id`, to be found in `tbl_system_messages`)
2) a story (accessed by the `story_id`, to be found in `tbl_stories`)
3) a chat (i.e set of questions to be asked, 1 to infinity, accessed by the `chat_id`, to be found in `tbl_chat`)

Hence, to add a new scenario to be asked to ChatGPT, one would decide which study it belongs to (from `tbl_studies`), the story (from `tbl_stories`), and the set of questions, or the `chat` (from `tbl_chats`), which would include the IDs of the questions to be asked (from `tbl_questions`).

##### Columns

1) `scenario_id`: the unique integer ID of the scenario
2) `scenario_code`: the unique code of the scenario
3) `study_id`: the ID of the study to which the scenario belongs
4) `story_id`: the ID of the story to be used in the scenario
5) `chat_id`: the ID of the collection of questions to be asked

#### tbl_stories

Apart from the obvious story content to be sent to ChatGPT, there are some pieces of information we wished to associate with each story. Namely, its name, language, ID common to both the English and Turkish versions, and finally category of ToM task (unexpected contents or unexpected transfer).

##### Columns

1) `story_id`: the unique ID of the story
2) `story_common_id`: the ID of the scenario common to different language versions of the same story
3) `story_category`: category of the ToM task (Unexpected Contents or Unexpected Transfer)
4) `story_name`: name of the story
5) `story_content`: the content of the story
6) `story_language`: the language of the story (English or Turkish)

#### tbl_chats

In the `tbl_scenarios` section, we had mentioned that a `scenario` is defined as

1) a `system message`
2) a `story`
3) a `chat` (a set of questions)

As we had discussed, the system message is associated with the `study_id`, to be found in `tbl_system_messages`; the story is associated with the `story_id`, found in `tbl_stories`, and finally the set of questions to be asked is found in this sheet, referred to as a `chat`, since it is a collection of questions, or collection of prompts. The individual questions to be included in a given `chat` are to be found in `tbl_questions`.

##### Columns

1) `chat_id`: the unique integer ID of the chat
2) `chat_name`: the unique name of the chat
3) `chat_language`: the language of the chat
4) `chat_has_fbv_zan`: whether the chat includes at least one prompt making use of the "zannetmek" Turkish false belief verb
5) `chat_has_fbv_san`: whether the chat includes at least one prompt making use of the "sanmak" Turkish false belief verb
6) `questions`: a comma and space (", ") seperated list of `question_id`s (to be found in `tbl_questions`)

#### tbl_questions

Here, one can find the individual questions to be asked, which are grouped as collections called `chat`s in `tbl_chats`.

##### Columns

1) `question_id`: the unique ID of the question
2) `question_common_id`: the ID common to different language versions of the same question
3) `question_content`: the content of the question
4) `question_options`: the possible options as an answer (which will be used if `with_options=True` while running the `notebooks/04_get_responses.ipynb` notebook)
5) `question_type`: the type of the question (closed-ended or open-ended)
6) `question_language`: the language of the question (English or Turkish)
7) `question_has_fbv_zan`: whether the question makes use of the "zannetmek" Turkish false belief verb
8) `question_has_fbv_zan`: whether the question makes use of the "sanmak" Turkish false belief verb
9) `question_tom_order`: the order of ToM being investigated (0 for reality and knowledge checks, 1 for first order beliefs, 2 for second order beliefs)
10) `question_tom_type`: the type of ToM being investigated (reality, knowledge, belief, thought, action, or explanation)

#### tbl_answers

Each question to be found in `tbl_questions` has a predefined correct answer, and this is the sheet where they are defined. With closed-ended questions (as defined in the question's `question_type` column), the correct answer is more straightforward, since it is a clear-cut, one word or phrase answer. With open-ended questions, we came up with an answer that addressed the question in the most logical manner possible, with no inferences or assumptions being used. We expected an answer similar to this one, even if not in the same words.

##### Columns

1) `answer_id`: the unique ID of the answer
2) `story_id`: the ID of the story (found in `tbl_stories`) with which the answer is associated 
3) `question_id`: the ID of the question (found in `tbl_questions`) with which the answer is associated 
4) `answer_correct`: the correct response to the given question, addressed at the given story
