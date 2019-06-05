# NHKの番組表をリスト表示するページ

```elm
import Browser
import Browser.Navigation as Nav
import Html exposing (..)
import Html.Attributes exposing (..)
import Url
import Url.Parser exposing (Parser, (</>), int, map, oneOf, s, string)
import Json.Decode exposing (Decoder, field, list, string, map2)
import Http
import Html.Events exposing (onInput)

-- MAIN


main : Program () Model Msg
main =
  Browser.application
    { init = init
    , view = view
    , update = update
    , subscriptions = subscriptions
    , onUrlChange = UrlChanged
    , onUrlRequest = LinkClicked
    }



-- MODEL


type alias Model =
  {
    areaCode: String
  , serviceCode: String
  , genreCode: String
  , spotList: List Spot
  }


init : () -> Url.Url -> Nav.Key -> ( Model, Cmd Msg )
init _ url key =
  let
    model = Model "010" "g1" "0000" []
  in
    ( model, getList model )



-- UPDATE


type Msg
  = LinkClicked Browser.UrlRequest
  | UrlChanged Url.Url
  | GotList (Result Http.Error (List Spot))
  | ChangeArea String
  | ChangeService String
  | ChangeGenre String


update : Msg -> Model -> ( Model, Cmd Msg )
update msg model =
  let _ = Debug.log "update" msg in
  case msg of
    LinkClicked urlRequest ->
      (model, Cmd.none)

    UrlChanged url ->
      (model, Cmd.none)

    GotList result ->
      case result of
        Ok spotList ->
          ({ model | spotList = spotList } , Cmd.none)

        Err _ ->
          (model, Cmd.none)
    ChangeArea code ->
      let
        newModel = { model | areaCode = code }
      in
        (newModel, getList newModel)

    ChangeService code ->
      let
        newModel = { model | serviceCode = code }
      in
        (newModel, getList newModel)

    ChangeGenre code ->
      let
        newModel = { model | genreCode = code }
      in
        (newModel, getList newModel)

-- SUBSCRIPTIONS


subscriptions : Model -> Sub Msg
subscriptions _ =
  Sub.none



-- VIEW


view : Model -> Browser.Document Msg
view model =
  { title = "Spot List"
  , body =
    [
      select [ onInput ChangeArea ] ( List.map viewOption areaList )
    , select [ onInput ChangeService ] ( List.map viewOption serviceList )
    , select [ onInput ChangeGenre ] ( List.map viewOption genreList )
    , ul [] ( List.map viewSpot model.spotList )
    ]
  }


viewSpot : Spot -> Html msg
viewSpot spot =
  li [] [ text (spot.name ++ spot.pref), a [] [ text "link" ]]


-- viewOption : Area -> Html msg
viewOption {code, name} =
  option [ value code ] [ text name ]


type alias Spot =
  { name : String
  , pref : String
  }


getList : Model -> Cmd Msg
getList model =
  Http.get
    { url = "https://api.nhk.or.jp/v2/pg/genre/" ++ model.areaCode ++"/" ++ model.serviceCode ++ "/" ++ model.genreCode ++ "/2019-06-05.json?key=apikey"
    , expect = Http.expectJson GotList (listDecoder model)
    }


listDecoder : Model -> Decoder (List Spot)
listDecoder model =
  field "list" ( field model.serviceCode (list spotDecoder))


spotDecoder : Decoder Spot
spotDecoder =
  map2 Spot
      (field "title" string)
      (field "subtitle" string)


type alias Area =
  {
    code: String
  , name: String
  }

areaList: List Area
areaList =
  [ Area "010" "札幌"
  , Area "011" "函館"
  , Area "012" "旭川"
  , Area "013" "帯広"
  , Area "014" "釧路"
  , Area "015" "北見"
  , Area "016" "室蘭"
  , Area "020" "青森"
  , Area "030" "盛岡"
  , Area "040" "仙台"
  , Area "050" "秋田"
  , Area "060" "山形"
  , Area "070" "福島"
  , Area "080" "水戸"
  , Area "090" "宇都宮"
  , Area "100" "前橋"
  , Area "110" "さいたま"
  , Area "120" "千葉"
  , Area "130" "東京"
  , Area "140" "横浜"
  , Area "150" "新潟"
  , Area "160" "富山"
  , Area "170" "金沢"
  , Area "180" "福井"
  , Area "190" "甲府"
  , Area "200" "長野"
  , Area "210" "岐阜"
  , Area "220" "静岡"
  , Area "230" "名古屋"
  , Area "240" "津"
  , Area "250" "大津"
  , Area "260" "京都"
  , Area "270" "大阪"
  , Area "280" "神戸"
  , Area "290" "奈良"
  , Area "300" "和歌山"
  , Area "310" "鳥取"
  , Area "320" "松江"
  , Area "330" "岡山"
  , Area "340" "広島"
  , Area "350" "山口"
  , Area "360" "徳島"
  , Area "370" "高松"
  , Area "380" "松山"
  , Area "390" "高知"
  , Area "400" "福岡"
  , Area "401" "北九州"
  , Area "410" "佐賀"
  , Area "420" "長崎"
  , Area "430" "熊本"
  , Area "440" "大分"
  , Area "450" "宮崎"
  , Area "460" "鹿児島"
  , Area "470" "沖縄"
  ]

type alias Service =
  {
    code: String
  , name: String
  }

serviceList: List Service
serviceList =
  [ Service "g1" "ＮＨＫ総合１"
  , Service "g2" "ＮＨＫ総合２"
  , Service "e1" "ＮＨＫＥテレ１"
  , Service "e2" "ＮＨＫＥテレ２"
  , Service "e3" "ＮＨＫＥテレ３"
  , Service "e4" "ＮＨＫワンセグ２"
  , Service "s1" "ＮＨＫＢＳ１"
  , Service "s2" "ＮＨＫＢＳ１(１０２ｃｈ)"
  , Service "s3" "ＮＨＫＢＳプレミアム"
  , Service "s4" "ＮＨＫＢＳプレミアム(１０４ｃｈ)"
  , Service "r1" "ＮＨＫラジオ第1"
  , Service "r2" "ＮＨＫラジオ第2"
  , Service "r3" "ＮＨＫＦＭ"
  , Service "n1" "ＮＨＫネットラジオ第1"
  , Service "n2" "ＮＨＫネットラジオ第2"
  , Service "n3" "ＮＨＫネットラジオＦＭ"
  , Service "tv" "テレビ全て"
  , Service "radio" "ラジオ全て"
  , Service "netradio" "ネットラジオ全て"
  ]


type alias Genre =
  {
    code: String
  , name: String
  }

genreList: List Genre
genreList =
  [ Genre "0000" "ニュース／報道(定時・総合)"
  , Genre "0100" "スポーツ(スポーツニュース)"
  , Genre "0205" "情報／ワイドショー(グルメ・料理)"
  , Genre "0300" "ドラマ(国内ドラマ)"
  , Genre "0409" "音楽(童謡・キッズ)"
  , Genre "0502" "バラエティ(トークバラエティ)"
  , Genre "0600" "映画(洋画)"
  , Genre "0700" "アニメ／特撮(国内アニメ)"
  , Genre "0800" "ドキュメンタリー／教養(社会・時事)"
  , Genre "0903" "劇場／公演(落語・演芸)"
  , Genre "1000" "趣味／教育(旅・釣り・アウトドア)"
  , Genre "1100" "福祉(高齢者)"
  ]

```
