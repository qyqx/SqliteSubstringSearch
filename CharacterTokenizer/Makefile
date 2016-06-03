LIB=-ldl -pthread -lsqlite3
BIN=sqlite3
FLAGS=-DSQLITE_ENABLE_FTS3 -DSQLITE_ENABLE_FTS3_PARENTHESIS -I"/home/qt/tmp/sqlite/src/sqlite-src-3130000/"


all: character_tokenizer.so

character_tokenizer.so: fts3_tokenizer.h character_tokenizer.c /home/qt/tmp/sqlite/src/sqlite-src-3130000/sqlite3.c
	@ gcc -fPIC -c character_tokenizer.c -o character_tokenizer.o
	@ gcc $(FLAGS) -shared -o character_tokenizer.so character_tokenizer.o $(LIB)
	@ test -e character_tokenizer.so && echo "[INFO] character_tokenizer.so is ok"

test: character_tokenizer.so 
	@ rm -rf ./test.db && ./$(BIN) test.db "CREATE VIRTUAL TABLE data USING fts3();" && echo "[INFO] fts3(fts4) is enabled" && rm -rf ./test.db
	@ rm -rf ./test.db && echo -e ".load $(PWD)/changyy \n CREATE VIRTUAL TABLE data USING fts3(tokenize=changyy);" | ./$(BIN) test.db && echo "[INFO] changyy_tokenizer is enabled" && rm -rf ./test.db
	@ rm -rf ./test.db && echo -e ".load $(PWD)/changyy \n CREATE VIRTUAL TABLE data USING fts3(tokenize=changyy); \n INSERT INTO data ('content') VALUES ('changyy changes changyys world'); \n SELECT SNIPPET(data) FROM data WHERE data MATCH 'changyy'; " | ./$(BIN) test.db && echo "[INFO] changyy_tokenizer: query 'changyy' done" && rm -rf ./test.db
	@ rm -rf ./test.db && echo -e ".load $(PWD)/changyy \n CREATE VIRTUAL TABLE data USING fts3(tokenize=changyy); \n INSERT INTO data ('content') VALUES ('changyy changes changyys world'); \n SELECT SNIPPET(data) FROM data WHERE data MATCH 'changes'; " | ./$(BIN) test.db && echo "[INFO] changyy_tokenizer: query 'changes' done" && rm -rf ./test.db
	@ rm -rf ./test.db && echo -e ".load $(PWD)/changyy \n CREATE VIRTUAL TABLE data USING fts3(tokenize=changyy); \n INSERT INTO data ('content') VALUES ('changyy changes changyys world'); \n SELECT SNIPPET(data) FROM data WHERE data MATCH 'changyys'; " | ./$(BIN) test.db && echo "[INFO] changyy_tokenizer: query 'changyys' done" && rm -rf ./test.db

clean:
	rm -rf ./$(BIN) ./test.db ./character_tokenizer.so ./*.o ./a.out  
