---
title: 깃허브 블로그 만들고 싶어요
type: docs
prev: /
next: docs/folder/
---

어떤 블로그 사이트를 쓸까 계속 고민했습니다. </br>
미디움, 티스토리, 깃 블로그 세 개가 후보군이었고 미디움과 티스토리 두 개를 병행하기로 결정했었습니다. </br>
그러나 미디움은 너무 글 작성이 불편했습니다. 가장 중요한 건 ****엔터가 안 쳐진다는 것.**** 

저는 글을 읽을 때 얼마나 편안한지가 중요한 사람인데, 엔터를 여러번 칠 수가 없어서 매우 불편했어요. 내가 글을 쓸 때 이렇게 보이겠지 의도한 바가 있는데 그게 잘 반영되지 않았어요. (사실 지금도 자간이나 엔터 반영이 잘 안되어서 불편해요. 이건 방법을 찾아보아야겠죠.)

어찌됐든 결론은, 깃허브 블로그를 만들어야겠다였습니다. 

우선 테마를 사용하기로 했습니다. 크게는 hugo, jllki가 있는데요. 저는 hugo를 사용하기로 했어요. 

우선 휴고 사이트에서 테마를 고릅니다. 그러면 끝난거나 다름없어요. 사실 전 애를 좀 먹긴했지만, 공식문서 보고 따라가면 정말 쉽답니다.

해당 [휴고 공식 문서](https://gohugo.io/getting-started/quick-start)를 띄워두고 천천히 따라가보시는 것도 추천드려요.

</br>
</br>

---

</br>

우선 **Hugo**와 **go**를 설치해줄게요. 저는 brew를 이용해 설치했어요.
```fallback
brew install hugo
```

그다음은 공식 문서의 순서를 차근히 따라갈거예요. 만약 테마에 getting started같은 설명글이 있다면 한번 정독하면 좋아요. 


```
hugo new site [폴더 이름]
```
휴고 사이트를 만들거예요.
`hugo new site [폴더 이름] --format=yaml` 이와 같이 format을 지정해줄 수도 있어요. hugo.toml 파일의 포맷이 yaml로 변경돼요. hugo.toml 파일은 설정 파일이라고 이해하면 돼요.

```
git init

git submodule add [테마 깃 사이트] themes/[테마 이름]
```
git submodule을 통해 테마를 적용시켜 줄 거예요.
나중에, 테마를 업데이트 하기 위해선 `git submodule update --remote` 해당 명령어를 사용해요.
```
echo "theme = ['테마 이름']" >> hugo.toml
```
그리고 hugo.yaml 파일에 가서 thema: [테마 이름]을 추가해요. 만약 테마에서 제공해주는 toml 파일이 있다면 그걸로 교체해요. 위 명령어는 toml 파일에 theme를 추가해주는 명령어예요. 직접 해도 되고 명령어를 사용해도 돼요
```
hugo server -D
```
명령어를 사용해 로컬에서 hugo server을 띄워요. 벌써 끝났어요. 잘 따라왔다면 http://localhost:1313/ 사이트에서 적용된 테마를 확인할 수 있어요.

</br>

이제는 로컬 뿐만 아니라 외부에서 볼 수 있도록 배포를 해줄거예요. 저는 깃 페이지를 사용했어요.
</br>
</br>
### 깃 페이지 배포
우선 레포지토리를 하나 생성해야 해요. `[깃허브아이디].github.io`라는 이름으로 레포를 생성해요. 이것은 저희의 도메인이 될 거예요. (돈을 주면 다른 도메인도 사용할 수 있어요. 📢)</br>
그리고 해당 레포지토리에 저희가 열심히 한 휴고 블로그를 올릴 거예요. 위에서 블로그를 만든 폴더에서 해당 명령어들을 실행해요. (new site)
```
git remote add origin [remote repository url]
git add .
git commit -m "commit message"
git push origin master
```
이렇게 하면 깃 레포지토리에 hugo 블로그를 구성하는 것들이 올라갈 거예요.


코드가 다 올라갔어요. 이제 깃 페이지를 사용해볼게요.

깃 레포지토리의 setting에 들어가요. </br>
왼쪽 탭의 pages에 들어가면 뭘 사용해 배포할지 선택할 수 있어요. 우리는 git action을 사용해 cicd를 할거예요. 해당 레포지토리에 푸시가 되면, 자동으로 웹사이트에 올라가게 해줄거예요.
제가 사용한 [work flow 파일](https://github.com/jongeuni/jongeuni.github.io/blob/master/.github/workflows/hugo.yml)이에요. (사용하실 때 base url을 변경해주세요.) 기본적으로 제공해주는 hugo work flow 파일도 있으니 그걸 사용하셔도 좋아요.

참고로 README 파일이 있다면 삭제 해주셔야해요.

여기까지 왔으면 배포가 잘 된걸 확인할 수 있을 거예요. 마음에 안 드는 부분이 있으면 조금조금 수정하면 돼요. 저도 날짜가 찍히지 않는것, 글 쓰는 게 불편한 것 두 가지가 마음에 안 들어요.</br>
모두 읽어주셔서 감사해요. 잘못되거나 추가되면 좋겠는 부분은 댓글 남겨주세요.
</br>
</br>
</br>
</br>
>💬 저는 이 글들을 참고했어요.</br>
[jekyll -> hugo 이전](https://blog.dslab.kr/jekyll-to-hugo/)</br>
[사용 테마 퀵스타트 가이드](https://arc.net/l/quote/uoswogyc)
