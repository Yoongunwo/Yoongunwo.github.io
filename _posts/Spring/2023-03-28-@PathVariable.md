---
title: "Spring MVC : @PathVariable"
categories:
  - Spring
toc: true
toc_label: "목차"
#toc_icon:
toc_sticky: true
#last_modified_at:
---

## 1. @PathVariable을 이요한 경로 변수 처리
다음은 ID가 10인 회원의 정보를 조회하기 위한 URL이다
> http ://localhost:8080/sp5-chap14/members/10

이 형식의 URL을 사용하면 각 회원마다 경로의 마지막 부분이 달라진다. 이렇게 경로의 일부가 고정되어 있지 않고 달라질 때 사용할 수 있는 것이 @PathVariable 애노테이션이다. @PathVariable 애노테이션을 사용하면 다음과 같은 방법으로 가변 경로를 처리할 수 있다.

```java
import org.springframework.web.bind.annoatation.PathVariable;
...

@Controller
public class MemberDetailController{
    private MemberDao memberDao;

    public void setMemberDao(MemberDao memberDao){
        this.memberDao = memberDao;
    }

    @GetMapping("/members/{id}")
    public String detail(@PathVariable("id") Long memId, Model model) {
        Member member = memberDao.selectById(memId);
        if(member == null){
            throw new MemberNotFoundException();
        }
        model.addAttribute("member", member);
        return "member/memberDetail";
    }
}
```
- 매핑 경로에 "{경로변수}" 와 같이 중괄호로 둘러 쌓인 부분을 **경로 변수**라고 부른다. "{경로 변수}"에 해당하는 값은 같은 경로 변수 이름을 지정한 @PathVariable 파라미터에 전달된다.\
- 12~13행의 경우 "/members/{id}"에서 {id}에 해당하는 부분의 경로 값을 @PathVariable("id")애노테이션이 적용된 memId
 파라미터에 전달된다.

- 예를 들어 요청 경로가 "/members/10"이면 {id}에 해당하는 "10"이 memId파라미터에 값으로 전달된다. memId 파라미터의 타입은 Long인데 이 경우 String 타입 값 "0"을 알맞게 Long타입으로 변환한다.



## Ref.
- 최범균, 스프링프로그래밍입문5, 가메출판사.
