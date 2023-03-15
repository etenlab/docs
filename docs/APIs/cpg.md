# CPG API

CPG API is service requests for CPG APP

## Data Synchronization

CPG API provides REST endpoints for syncing data between the server and clients (CPG APPs).
That required doing schema changes:

- all data has globally unique ids (`uuid`s): both server-side and client-side. Hence, incremental IDs are not needed.
- all syncable server-side data has `updated_at` field (`timestamp`)
- all syncable client-side data has `sync_layer` field (`int`)

### From Server

REST endpoint: `/sync/from-server`

Whenever a row mutated (being created, updated, deleted) we update its `updated_at` column.
When a client tries to sync, it provides `last-sync` url parameter with ISO string indicating his last sync timestamp (from server). This value is used to filter out necessary rows in all syncable tables (`>= :lastSync`).
When a client receives data, it should not set `sync_layer` field (or set to something smaller then the actual sync layer is) so the data won't be synced back to server alter.

### To Server

REST endpoint: `/sync/to-server`

On the client-side, we cannot rely on time values. That's why we use incrementing integer to mark mutated data.
Whenever a row is mutated (being created, updated, deleted) we update its `sync_layer` column to be equal to current global sync layer value (stored in cookies).
Every attempt to synchronize should take the global sync layer value (let's name it `currentSyncLayer`) and increment it before reading data from the database.
Data for synchronization should be taken by filter:
`:lastSyncLayer` < `sync_layer` \<\= `:currentSyncLayer`
After a sync session is completed, `lastSyncLayer` = `currentSyncLayer`.
