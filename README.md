# iRODS client comparison

This repo serves as a record of comparisons of various iRODS clients against one another primarily to help npg make decisions about how feasible it would be to make use of those clients to replace the c clients in various situations.

It contains a script that will run the clients that we use most frequently on each of a set of files provided (by the user) in a specified directory.  For testing behaviour of the clients when putting or getting to a location that already has a file present, the script also requires paths to files provided for this purpose.  

## gocommands/go-irodsclient

- No problems with replication on a local (docker) server with replication enabled.

- Still in active (v0.x) development, so bugs are being both fixed and introduced relatively regularly, and the api is not stable.

- v0.9.10>gocmd<=0.9.14 (current at time of writing) put fails against dev server for reasons that I have not been able to identify or replicate on a server running through docker.  Putting results in an error: FILE_TOO_LARGE on any size of file. v0.9.9 is prior to additions such as calculating checksums and renaming files when putting or getting.

- There is also currently a seg fault in the code that checks the encryption flag if no encryption flag (neither --encrypt or --no_encrypt) is set - pr submitted to fix this as the cause was easy to find.

- Some functions such as chmod-ing and querying metadata, while present in the go-irodsclient api, are not implemented in gocmd (see https://github.com/mksanger/go-baton for simple implemenations based on baton).

- Blocking prompt when attempting to overwrite a file with put or get but force flag not set, not ideal for scripting (no "don't force" flag).

- Cross zone metadata searches are not possible (and not expecting to be added soon https://github.com/cyverse/go-irodsclient/issues/40)


### Conclusions

- The go irods clients are not ready for production scale use at the time of writing, but could be useful as a more portable client set once they are further in development.