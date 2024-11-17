# RecomMind

## Overview
*RecomMind* is a movie recommendation dialogue dataset in Japanese.
This dataset includes annotations of the seeker's internal state (knowledge state and interest state specifically) at the entity level.
The dataset contains two types of labels for each entity:
- *First-person labels*: Annotated by the seeker to express their own internal state.
- *Second-person labels*: Annotated by the recommender, reflecting their estimation of the seeker's internal state.

## Files

The data for each dialogue is stored in the `dialogues/{train,valid,test}/` directories, with each file following the format `<dialogue_id>.json`.
Additionally, the data for each movie is stored in the `movies/` directory, with each file following the format `<movie_id>.json`.

## Data Format

### Dialogue Data
Each dialogue file contains the following format:
```text
{
  "dialog_id": "5f9b25f7-04ec-4305-8740-dad54070226b",
  "created_timestamp": "2023-02-22T17:58:39.456786+09:00",
  "archived_timestamp": "2023-02-22T18:31:50.273093+09:00",
  "recommender": {
    "name": "037",
    "questionnaire": {
      "knowledge": 1,
      "enjoyment": 5,
      "recommendation_success": 5,
      "movie_num": 5
    }
  },
  "seeker": {
    "name": "003",
    "questionnaire": {
      "knowledge": 1,
      "enjoyment": 3,
      "recommendation_success": 3,
      "movie_num": 3
    }
  "utterances": [
    ...
    {
      "id": "1",
      "speaker": "recommender",
      "text": "こんばんは！よろしくお願いいたします。",
      "timestamp": "2023-02-22T18:02:08.077777+09:00",
      "internal_states": [],
      "searches": [],
      "checked": []
    },
    {
      "id": "2",
      "speaker": "seeker",
      "text": "普段は、邦画を見ることが多いです。\nホラーは苦手で、ＳＦ系もあまり好みではありません。\nおすすめがあれば、教えていただけますか？",
      "timestamp": "2023-02-22T18:03:23.257216+09:00",
      "internal_states": [
        {
          "id": "0",
          "text": "邦画を見ること",
          "knowledge": {
            "recommender": "あり",
            "seeker": "あり"
          },
          "interest": {
            "recommender": "あり",
            "seeker": "あり"
          }
        },
        {
          "id": "1",
          "text": "ホラー",
          "knowledge": {
            "recommender": "あり",
            "seeker": "あり"
          },
          "interest": {
            "recommender": "なし",
            "seeker": "なし"
          }
        },
        {
          "id": "2",
          "text": "ＳＦ系",
          "knowledge": {
            "recommender": "あり",
            "seeker": "あり"
          },
          "interest": {
            "recommender": "なし",
            "seeker": "なし"
          }
        }
      ]
    },
    ...
  ]
}
```

- `dialog_id`: A unique identifier for the dialogue.
- `created_timestamp`: The timestamp when the dialogue was created.
- `archived_timestamp`: The timestamp when the dialogue was archived.
- `recommender`: Information about the recommender.
  - `name`: The name of the recommender. We use the number for anonymization.
  - `questionnaire`: The recommender's questionnaire responses. All questions are answered on a 5-point Likert scale, with five being the best and one being the worst.
    - `knowledge`: `Do you know the movie you recommended?`
    - `enjoyment`: `Did you enjoy the dialogue?`
    - `recommendation_success`: `Do you think you have recommended the
movie well?`
    - `movie_num`: `How many movies do you watch per year?`
- `seeker`: Information about the seeker.
  - `name`: The name of the seeker. We use the number for anonymization.
  - `questionnaire`: The seeker's questionnaire responses. All questions are answered on a 5-point Likert scale, with five being the best and one being the worst.
    - `knowledge`: `Do you know the movie that was
recommended?`
    - `enjoyment`: `Did you enjoy the dialogue?`
    - `recommendation_success`: `Do you want to watch the recommended
movie?`
    - `movie_num`: `How many movies do you watch per year?`
- `utterances`: A list of dialogue turns.
  - `id`: A unique identifier for the utterance.
  - `speaker`: The speaker of the utterance (`recommender` or `seeker`).
  - `text`: The text of the utterance.
  - `timestamp`: The timestamp when the utterance was made.
  - `internal_states`: A list of the seeker's internal states at the entity level.
    - `id`: A unique identifier for the entity.
    - `text`: The text of the entity.
    - `knowledge`: knowledge state label for the entity.
      - `recommender`: Second-person label. The option is {`あり`, `どちらとも言えない`, `なし`, `不適切`}. 
      - `seeker`: First-person label. The option is {`あり`, `どちらとも言えない`, `なし`, `不適切`}.
    - `interest`: The seeker's and recommender's interest in the entity.
      - `recommender`: Second-person label. The option is {`あり`, `どちらとも言えない`, `なし`, `不適切`}.
      - `seeker`: First-person label. The option is {`あり`, `どちらとも言えない`, `なし`, `不適切`}.
  - `searches`: Information about movie searches during the utterance.
    - `timestamp`: The timestamp when the search was made.
    - `query`: The search query.
    - `genres`: The genres the recommender searched for.
    - `results`: The search results. A list of movie IDs.
  - `checked`: A list of external knowledge sources checked by the recommender.

### Movie Data
Each movie file contains the following format:
```text
{
  "id": "359899",
  "title": {
    "type": "タイトル",
    "text": "怪盗グルーのミニオン大脱走",
    "wiki_text": "怪盗グルーのミニオン大脱走"
  },
  "summary": {
    "type": "概要",
    "text": "『怪盗グルーのミニオン大脱走』（かいとうグルーのミニオンだいだっそう、英: Despicable Me 3）は、2017年公開のアメリカ合衆国の3Dコンピュータアニメーション・コメディ映画。「怪盗グル
ー」シリーズの第3作目である。"
  },
  "year": {
    "type": "公開年",
    "text": "2017年7月21日"
  },
  "time": {
    "type": "上映時間",
    "text": "90分"
  },
  "director": {
    "type": "監督",
    "text": "カイル・バルダ",
    "other_titles": ["359899"]
  },
  "cast": [
    {
      "type": "キャスト",
      "text": "スティーヴ・カレル",
      "other_titles": ["336808", ...]
    }
  ],
  "original": {
    "type": "原作",
    "text": ""
  },
  "theme_song": {
    "type": "主題歌",
    "text": ""
  },
  "country": {
    "type": "製作国",
    "text": "アメリカ合衆国"
  },
  "box_office": {
    "type": "興行収入",
    "text": "$1,034,799,409"
  },
  "genre": [
    {
      "type": "ジャンル",
      "text": "アドベンチャー"
    },
    ...
  ],
  "review": [
    {
      "type": "レビュー",
      "text": "まだまだお話が続きそうな感じが逆にこどもの好奇心を誘うようでよかったです。"
    },
    ...
  ],
  "plot": [
    {
      "type": "あらすじ",
      "text": "晴れて結婚したグルーとルーシーの前に、新たな敵バルタザール・ブラットが現れる。"
    },
    ...
  ]
}
```

- `id`: A unique identifier for the movie.
- `title`: Information about the movie title.
  - `type`: The type of the title.
  - `text`: The text of the title.
  - `wiki_text`: The text of the title in Wikipedia.
- `summary`: Information about the movie summary.
  - `type`: The type of the summary.
  - `text`: The text of the summary.
- `year`: Information about the movie release year.
  - `type`: The type of the release year.
  - `text`: The text of the release year.
- `time`: Information about the movie runtime.
  - `type`: The type of the runtime.
  - `text`: The text of the runtime.
- `director`: Information about the movie director.
  - `type`: The type of the director.
  - `text`: The text of the director.
  - `other_titles`: A list of other movie IDs directed by the same director.
- `cast`: A list of information about the movie cast.
  - `type`: The type of the cast.
  - `text`: The text of the cast.
  - `other_titles`: A list of other movie IDs featuring the same cast.
- `original`: Information about the movie original work.
  - `type`: The type of the original work.
  - `text`: The text of the original work.
- `theme_song`: Information about the movie theme song.
  - `type`: The type of the theme song.
  - `text`: The text of the theme song.
- `country`: Information about the movie production country.
  - `type`: The type of the production country.
  - `text`: The text of the production country.
- `box_office`: Information about the movie box office.
  - `type`: The type of the box office.
  - `text`: The text of the box office.
- `genre`: A list of information about the movie genre.
  - `type`: The type of the genre.
  - `text`: The text of the genre.
- `review`: A list of information about the movie review.
  - `type`: The type of the review.
  - `text`: The text of the review.
- `plot`: A list if information about the movie plot.
  - `type`: The type of the plot.
  - `text`: The text of the plot.


## References

- Takashi Kodama, Hirokazu Kiyomaru, Yin Jou Huang, and Sadao Kurohashi: RecomMind: Movie Recommendation Dialogue with Seeker’s Internal State, In Proceedings of the Second Workshop on Social Influence in Conversations (SICon 2024), pp.46-63, Miami, Florida, USA, 2024. https://aclanthology.org/2024.sicon-1.4/

##  Acknowledgment
The construction of this dataset was supported by NII CRIS collaborative research program operated by NII CRIS and LINE Corporation. This work was also supported by JST, CREST Grant Number JPMJCR20D2, Japan and JSPS KAKENHI Grant Number JP22J15317.

## License
CC-BY-SA 4.0
