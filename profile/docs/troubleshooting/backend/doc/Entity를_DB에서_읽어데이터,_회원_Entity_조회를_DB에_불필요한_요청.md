# Entity를 DB에서 읽어데이터, 회원 Entity 조회를 DB에 불필요한 요청
작성자: 박강락

## 1. 문제 상황

- 댓글 Entity를 DB에서 읽어온 후 회원 Entity 조회를 DB에 추가적으로 요청하여 댓글 페이지 생성 시 작성자 수 만큼 추가적인 DB 통신이 이루어지고 있음

```
@Transactional(readOnly = true)
  public Page<CommentResponse> readComments(long gatheringId, Pageable pageable) {

    Pageable adjustedPageable = adjustPageable(pageable);
    Page<Comment> comments = commentRepository.findCommentsByGatheringId(
        gatheringId, adjustedPageable);

    List<CommentResponse> commentResponses = new ArrayList<>();
    for (Comment comment : comments) {
      Optional<Member> writer = memberRepository.findById(comment.getMemberId());
      writer.ifPresent(member -> commentResponses.add(
	      CommentResponse.fromEntity(comment, member.getNickName())));
           }

    return new PageImpl<CommentResponse>(commentResponses, adjustedPageable,
        commentResponses.size());

```

## 2. 원인

- 댓글 Entity와 회원 Entity를 별도로 요청하고 있음

## 3. 해결 방안

- JPA 엔티티 매핑 기능 사용하여 회원 Entity 매핑
- 리포지터리 클래스에서 @EntityGraph 어노테이션 사용하여 댓글과 회원 정보를 fetch join하여 한번의 DB 통신만 수행

```java
@EntityGraph(attributePaths = "writer")
  Page<Comment> findCommentsByGatheringId(long gatheringId, Pageable pageable);
```