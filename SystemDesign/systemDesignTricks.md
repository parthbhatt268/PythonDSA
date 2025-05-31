1. If you want to have more write than read you would likely use casandra Db cause its optimised for that.
2. Cassandra DB is noSQL data that si optimised for heavy writes
3. If you are design like living shairng doc  you are liskley going to use the Websocket
4. and you can procy websocket service as well and you can also have it got throught the API gateway like any other REST api.


## Google Docs deisgn
=> You have some way to handle the conflicts when you are creating live sharing code/word platform, so to solve conflicts you have
1. Last write wins
2. Operational transform (meaning when two user at cleint side edit at the exact same place we descide at service who wrote first and then the second user messgae is shifted a bit to later part of senetsece boz user 1 editted first and this is noted by the serve and the server timestamp(i.e. whendid server receved the suer 1 or user 2 messge (just to avoid if user had weak internet r soemthing we decided whi reach first based on who's message reached t oseerver first)))
3. send the whole updated document from cleint with teh update in it  (to heavy operation)
4. Send only the edits (issue here is we are not sure how to handle the conflitcs then hence we chooose operational Transfrom (option 2 in this list))
5. CRDT - conflict free replication Data types