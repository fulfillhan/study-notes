# API 개발 주의사항
# ✔️회원 등록

```
@PostMapping("/api/V1/members")
public CreateMemberResponse saveMemberV1(@RequestBody @Valid Member member){
    Long id = memberService.join(member);
    return new CreateMemberResponse(id);
}

@Data
static class CreateMemberResponse {
    private Long id;
    public CreateMemberResponse(Long name) {
        this.id = id;
        
        
    }
}
```
- 프레젠테이션 계층의 saveMemberV1() 의 파라미터 요청 값으로 엔티티가 직접 사용되었는데 검증 로직이 들어가있다.

# ✔️회원 조회
```
@GetMapping("/api/V1/members")
    public List<Member> membersV1(){
        return memberService.findMembers();
    }
```
- 응답 값으로 `List<Member>` 엔티티를 직접 노출했다.

# 🔥문제점
상위의 코드의 문제는 무엇일까?
-> 엔티티를 직접 노출하게 된 것이다.

## 엔티티를 직접 사용한 것이 왜 문제일까?
1. 해당 클래스와 엔티티 객체는 강한 연관관계를 갖게된다. 결합성이 높아진다는 것이다.
- 실제 개발에서는 API는 정말 다양하게 만들어질텐데, 한 엔티티에 각각의 API 요구사항이 다 담길 수 없다.
  이는 엔티티가 변경되면 API스펙도 변경되는 문제를 발생하게 된다.

2. 엔티티를 직접 파라미터로 사용하면 어떤 속성이 넘어오는지 알 수 없다.

⭐여로모로 `사이드 이펙트` 발생한다!!

**⭐추가로 회원 조회시 컬렉션을 직접 반환하면 향후 API 스펙을 변경하기 어렵다.-> 별도의 Result 클래스와 같이 리스트를 감싸서 클래스 생성해서 해결한다.**
# 해결책
API 스펙에 맞는 DTO 객체를 생성해서 사용해라!
# ✔️회원 등록
```
   @PostMapping("/api/V2/members")
    public CreateMemberResponse saveMemberV2(@RequestBody @Valid CreateMemberRequest request){
        Member member = new Member();
        member.setName(request.getName());

        Long id = memberService.join(member);
        return new CreateMemberResponse(id);
    }
    
    @Data
    static class CreateMemberRequest{
        @NotEmpty  //엔티티가 아닌 dto에서 검증설정을 할 수 있다.
        private String name;

    }
```
# ✔️회원 조회
```
 @GetMapping("/api/V2/members")
    public Result membersV2(){

        //엔티티 - > dto 로 변환해라
        List<Member> findMembers = memberService.findMembers();
        List<MemberDto> collect = new ArrayList<>();
        for (Member findMember : findMembers) {
            MemberDto memberDto = new MemberDto(findMember.getName());
            collect.add(memberDto);
        }
        return new Result(collect);
    }
    
    //리스트를 감싸서 클래스 생성한다. 컬렉션을 직접 반환하지 않기!
     @Data
    @AllArgsConstructor
    static class Result<T> {
        private T data;
    }
```
- 엔티티와 프레젠테이션 객체를 분리한다.
- 엔티티와 API스펙을 명확하게 분리 할 수 있다.
- API 변경과 엔티티 변경은 서로에게 영향을 미치지 않는다.

⭐⭐엔티티를 외부에 노출하거나 파라미터로 받는 것은 하지말자!!!!!!!!

