## JPA - Cascade & OrphanRemoval

#### 내가 겪은 에러

부모 엔티티에서 자식 엔티티 몇 개를 forEach를 돌려 삭제하는 과정에서

부모 엔티티를 save하는 순간 이미 삭제한 자식 엔티티의 id로 select query가 실행되며   

JpaObjectRetrievalFailureException이 발생했다.   



### 영속성 전이

어떤 엔티티를 영속 상태로 만들 때, 연관된 엔티티들도 함께 영속 상태로 만들어준다.

예를 들어, 부모 엔티티에서 자식 엔티티들을 컬렉션으로 갖고 있을 경우에

자식 엔티티를 부모 엔티티에 추가하는 것만으로도 영속성을 띄게 만들어 준다.



### CascadeType

ALL,  PERSIST,  MERGE,  REMOVE,  REFRESH,  DETACH

말그대로다.



### OrphanRemoval

부모를 잃은 자식을 DB에서도 삭제해준다.



#### 내가 겪은 에러의 원인은?

JPA에서 delete를 할 때, 조건에 해당하는 레코드를 바로 삭제하는 게 아니라   

delete할 엔티티 및 그와 연관된 엔티티들을 먼저 select한 뒤, id를 이용해 하나하나씩 delete 처리를 한다.   

<jpa delete 과정 : https://jojoldu.tistory.com/235>

따라서, forEach문으로 delete를 할 경우에 각각의 자식객체가 갖고 있는 부모객체 필드가 이미 삭제된 엔티티를 포함한 상태로 유지되고 있어서 참조가 남아있는 상태로 삭제를 했기 때문에 1번 삭제 이후 2번 삭제 처리 때 없는 엔티티를 select 하여 delete하려다가 select가 불가능 해서 에러가 발생한 것이다.

이를 해결하기 위해서 각각의 자식 엔티티들을 직접 레파지토리에서 삭제하도록 변경했다.

