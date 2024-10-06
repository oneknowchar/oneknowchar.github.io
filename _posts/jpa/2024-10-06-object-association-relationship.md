---
title: '객체 연관관계(양방향 연관관계, 편의 메서드)'
excerpt: 'Object Association Relationship'

published: true

categories: jpa
tags: jpa

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

[이해 될때까지 보면 된다. 인프런 강의 link click](https://www.inflearn.com/course/lecture?courseSlug=ORM-JPA-Basic&unitId=21698&subtitleLanguage=ko&tab=curriculum&category=questionDetail&q=1324577)

# 객체 연관관계란?

**RDBS** 에서, team 과 member의 관계(1:N)는 memberId(FK)로 결정 됩니다.

그렇기 때문에 team과 member는 쿼리 작성 시 memberId만 잘 신경 쓴다면 연관 관계에 대헤서 크게 신경쓸 필요가 없습니다.

하지만 **OOP**의 경우 참조를 통하여 객체 관계를 조회하기 때문에  
객체가 또다른 객체를 참조하고 있어야 하는게 맞지,

필드값으로 이렇게 저렇게 는 OOP스러운 설계가 아닙니다.

## RDBS 스러운 문제...

```
public class Member{
	private Long id;
	private String member;
	private Long teamId;		// 굉장히 RDBS 스럽다
}
```

객체 안에 다른 객체의 필드(FK)값을 가지고~~~  
그 FK가지고 DB 조회를 해서~~~  
조회된 데이터를 다른 객체에 set set set~~~

이건 **OOP** 스럽지 않습니다.

RDBS스러운걸 OOP스럽게 해결하는 방법이 JPA엔 존재합니다.

# OOP 스럽게 문제 해결

## 빠른 정답

```
@getter
@setter
@Entity
public class Member{
	@Id @GeneratedValue
	private Long id;
	private String name;

	@ManyToOne
	@JoinColumn(name = "TEAM_ID")
	private Team team;				// OOP스러운 해결!

	//연관관계 편의 메서드
	public void chageTeam(Team team) {
		this.team = team;
		team.getMembers().add(this);
	}
}

@Entity
@getter
@setter
public class Team{
	@Id @GeneratedValue
	private Long id;

	private String name;
	@OneToMany(mappedBy = "team")	//Member 클래스의 매핑될 변수명
	private List<Member> members = new ArrayList<>();	//null방지 초기화
}
```

# 결론

0. 단방향 매핑만으로도 이미 연관관계 매핑은 끝입니다. 단방향 매핑을 잘 하고 양뱡향은 **필요할때 추가**해도 됩니다. 조회 기능이 전부이기 때문에 테이블에 영향을 주지 않습니다.

1. 무조건 연관 관계의 주인은 1:N 관계에서 **N이 되는 객체**로 설정하면 됩니다. 편의메서드도 상황에 따라 다르겠지만  
   N이 되는 객체, FK 의 주인이 되는 객체에 설정을 기본으로 잡읍니다.

2. 편의메서드 생성으로 순수객체 상태를 고려해서 항상 양쪽에 값을 설정합니다.(편의매서드)

3. 양방향 매핑시 무한 루프를 조심합니다.(toString(), lombok, JSON libraries)

# 주의사항

toString() 메서드 사용시, 양방향 무한 호출이 되니 주의하세요.

Entity 클래스를 컨트롤러 리턴으로 사용하지 말고 DTO따로 생성해서 만들면
JSON라이브러리가 직렬화 할때 문제의 여지가 없습니다.

양방향 관계에서 항상 두 케이스의 무한 호출에 주의하면 어려울게 없습니다.

---

여기까지 QueryDsl 세팅과 gradle build, 간단한 querydsql 실행까지 다뤄봤습니다.
