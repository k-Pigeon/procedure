create table books(
	id number(2) primary key,
	name varchar2(20) not null,
	write varchar2(20) not null,
	price number(10) default 0,
	genre varchar2(20) not null,
	publisher varchar2(20) not null,
	cnt number(10) default 0
);


create table books_bak as select * from books;

create table books_bak as select * from books where 1=2;


create sequence books_seq
start with 1
increment by 1
nocycle;


insert into books
values(books_seq.nextval, '홍길동전', '허균', 300, '고선소설', '성일출판사', 2);
insert into books
values(books_seq.nextval, '레미제라블', '빅토르 위고', 900, '소설', '빅토르출판사', 5);


create or replace procedure book_bak_proc
(
    BOOK_ID IN BOOKS.ID%TYPE
)
IS
    CURSOR CSOR IS SELECT ID, NAME, WRITER, PRICE, GENRE, PUBLISHER, CNT FROM BOOKS WHERE ID = BOOK_ID;
    B_ID BOOKS.ID%TYPE;
    B_NAME BOOKS.NAME%TYPE;
    B_WRITER BOOKS.WRITER%TYPE;
    B_PRICE BOOKS.PRICE%TYPE;
    B_GENRE BOOKS.GENRE%TYPE;
    B_PUBLISHER BOOKS.PUBLISHER%TYPE;
    B_CNT BOOKS.CNT%TYPE;
    
BEGIN
    OPEN CSOR;
        LOOP
            FETCH CSOR INTO B_ID, B_NAME, B_WRITER, B_PRICE, B_GENRE, B_PUBLISHER, B_CNT;
            EXIT WHEN CSOR%NOTFOUND;
                INSERT INTO BOOKS_BAK (ID, NAME, WRITER, PRICE, GENRE, PUBLISHER, CNT)
                VALUES (B_ID, B_NAME, B_WRITER, B_PRICE, B_GENRE, B_PUBLISHER, B_CNT);
                COMMIT;
        END LOOP;
    CLOSE CSOR;
    
END book_bak_proc;
/
