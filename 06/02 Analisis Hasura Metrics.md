# Hasura Metrics Explanation

Metrics di bawah ini dihasilkan oleh Hasura dan digunakan untuk memantau performa serta aktivitas dari berbagai komponen di dalamnya, seperti permintaan HTTP, langganan, event triggers, dan OpenTelemetry.

## Metrics List

1. **`hasura_action_request_bytes_total`**: Total ukuran badan permintaan HTTP yang dikirim melalui actions (dalam byte). Ini menunjukkan berapa banyak data yang dikirim saat menggunakan Hasura actions.
   
2. **`hasura_action_response_bytes_total`**: Total ukuran badan respons HTTP yang diterima melalui actions (dalam byte). Ini menunjukkan jumlah data yang diterima dari Hasura actions.

3. **`hasura_active_subscription_pollers`**: Jumlah pollers langganan aktif yang sedang berjalan. Terdapat dua jenis langganan, yaitu:
   - `live-query`: Langganan live-query yang berjalan.
   - `streaming`: Langganan streaming yang berjalan.

4. **`hasura_active_subscription_pollers_in_error_state`**: Jumlah pollers langganan aktif yang berada dalam keadaan error. Ini juga mencakup kedua jenis langganan (`live-query` dan `streaming`).

5. **`hasura_cache_request_count`**: Jumlah total permintaan cache lookup. Ini memberikan status apakah cache tersebut "hit" (data ditemukan) atau "miss" (data tidak ditemukan).

6. **`hasura_cron_events_invocation_total`**: Jumlah total cron events yang dipanggil. Dapat dilihat statusnya apakah "failed" (gagal) atau "success" (berhasil).

7. **`hasura_cron_events_processed_total`**: Jumlah total cron events yang diproses. Ini menunjukkan berapa banyak event yang telah diolah oleh Hasura, dengan status "failed" atau "success".

8. **`hasura_event_fetch_time_per_batch_seconds`**: Latensi waktu pengambilan batch events dari database, dalam detik. Ini memberikan distribusi waktu dengan beberapa rentang (buckets), seperti 1.0e-4 detik, 3.0 detik, dll.

9. **`hasura_event_trigger_http_workers`**: Jumlah pekerja HTTP event trigger yang aktif saat ini. Metrik ini menunjukkan jumlah worker yang sedang aktif mengirim event trigger.

10. **`hasura_event_trigger_request_bytes_total`**: Total ukuran badan permintaan HTTP yang dikirim melalui event triggers (dalam byte).

11. **`hasura_event_trigger_response_bytes_total`**: Total ukuran badan respons HTTP yang diterima melalui event triggers (dalam byte).

12. **`hasura_events_fetched_per_batch`**: Jumlah events yang diambil dalam satu batch.

13. **`hasura_graphql_execution_time_seconds`**: Waktu eksekusi untuk permintaan GraphQL yang berhasil (tidak termasuk subscriptions). Ini juga dibagi berdasarkan jenis operasi:
    - `mutation`: Waktu eksekusi untuk permintaan mutasi.
    - `query`: Waktu eksekusi untuk permintaan query.
    Terdapat beberapa rentang waktu (buckets) yang menunjukkan durasi eksekusi, dan total waktu eksekusi (`sum`) serta jumlah eksekusi (`count`) dicatat.

14. **`hasura_graphql_requests_total`**: Jumlah permintaan GraphQL yang diterima, dengan status respons dan jenis operasi (seperti `query`, `mutation`).

15. **`hasura_http_connections`**: Jumlah koneksi HTTP aktif saat ini (tidak termasuk WebSocket connections).

16. **`hasura_http_request_bytes_total`**: Total ukuran badan permintaan HTTP yang diterima oleh server HTTP Hasura (dalam byte).

17. **`hasura_http_response_bytes_total`**: Total ukuran badan respons HTTP yang dikirim oleh server HTTP Hasura (dalam byte).

18. **`hasura_metadata_resource_version`**: Versi metadata sumber daya saat ini.

19. **`hasura_oneoff_events_invocation_total`**: Jumlah total one-off events yang dipanggil. Tercatat dengan status "failed" atau "success".

20. **`hasura_oneoff_events_processed_total`**: Jumlah total one-off events yang diproses. Ini menunjukkan berapa banyak event yang telah selesai diolah.

21. **`hasura_otel_dropped_logs`**: Jumlah total log yang dibuang karena volume log yang tinggi. Ini termasuk alasan-alasan seperti `buffer_full` (penuh) atau `send_failed` (gagal dikirim).

22. **`hasura_otel_dropped_spans`**: Jumlah total trace spans yang dibuang karena volume trace yang tinggi. Ini juga mencakup alasan seperti `buffer_full` atau `send_failed`.

23. **`hasura_otel_sent_logs`**: Jumlah total log yang berhasil diekspor melalui OpenTelemetry.

24. **`hasura_otel_sent_spans`**: Jumlah total trace spans yang berhasil diekspor melalui OpenTelemetry.

25. **`hasura_scheduled_trigger_request_bytes_total`**: Total ukuran badan permintaan HTTP yang dikirim melalui scheduled triggers (dalam byte).

26. **`hasura_scheduled_trigger_response_bytes_total`**: Total ukuran badan respons HTTP yang diterima melalui scheduled triggers (dalam byte).

27. **`hasura_websocket_connections`**: Jumlah koneksi WebSocket aktif saat ini.

28. **`hasura_websocket_message_queue_time`**: Waktu (dalam detik) yang diperlukan agar pesan WebSocket tetap ada di antrean sebelum diproses oleh engine GraphQL Hasura.

29. **`hasura_websocket_message_write_time`**: Waktu yang diambil untuk menulis pesan WebSocket ke dalam buffer pengiriman TCP.

30. **`hasura_websocket_messages_received_bytes_total`**: Total ukuran pesan WebSocket yang diterima (dalam byte).

Metrik-metrik ini memungkinkan Anda untuk memantau berbagai aspek dari performa dan aktivitas Hasura, baik itu terkait GraphQL, event triggers, ataupun WebSocket.
"""

Aku sudah melakukan GET Hasura Metrics dan berikut metricsnya.

```text
# HELP hasura_action_request_bytes_total Total size of HTTP request bodies sent via actions (experimental)
# TYPE hasura_action_request_bytes_total counter
hasura_action_request_bytes_total 0.0

# HELP hasura_action_response_bytes_total Total size of HTTP response bodies received via actions (experimental)
# TYPE hasura_action_response_bytes_total counter
hasura_action_response_bytes_total 0.0

# HELP hasura_active_subscription_pollers Current active number of subscription pollers running
# TYPE hasura_active_subscription_pollers gauge
hasura_active_subscription_pollers{subscription_kind="live-query"} 0.0
hasura_active_subscription_pollers{subscription_kind="streaming"} 0.0

# HELP hasura_active_subscription_pollers_in_error_state Current active number of subscription pollers in the error state
# TYPE hasura_active_subscription_pollers_in_error_state gauge
hasura_active_subscription_pollers_in_error_state{subscription_kind="live-query"} 0.0
hasura_active_subscription_pollers_in_error_state{subscription_kind="streaming"} 0.0

# HELP hasura_cache_request_count Total number of incoming requests for cache lookup
# TYPE hasura_cache_request_count counter
hasura_cache_request_count{status="hit"} 0.0
hasura_cache_request_count{status="miss"} 0.0

# HELP hasura_cron_events_invocation_total Total number of cron events invoked
# TYPE hasura_cron_events_invocation_total counter
hasura_cron_events_invocation_total{status="failed"} 0.0
hasura_cron_events_invocation_total{status="success"} 0.0

# HELP hasura_cron_events_processed_total Total number of cron events processed
# TYPE hasura_cron_events_processed_total counter
hasura_cron_events_processed_total{status="failed"} 0.0
hasura_cron_events_processed_total{status="success"} 0.0

# HELP hasura_event_fetch_time_per_batch_seconds Latency of fetching a batch of events from the database
# TYPE hasura_event_fetch_time_per_batch_seconds histogram
hasura_event_fetch_time_per_batch_seconds_bucket{le="1.0e-4"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="3.0e-4"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="1.0e-3"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="3.0e-3"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="1.0e-2"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="3.0e-2"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="0.1"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="0.3"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="1.0"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="3.0"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="10.0"} 0
hasura_event_fetch_time_per_batch_seconds_bucket{le="+Inf"} 0
hasura_event_fetch_time_per_batch_seconds_sum 0.0
hasura_event_fetch_time_per_batch_seconds_count 0

# HELP hasura_event_trigger_http_workers Current number of active event trigger HTTP workers
# TYPE hasura_event_trigger_http_workers gauge
hasura_event_trigger_http_workers 0.0

# HELP hasura_event_trigger_request_bytes_total Total size of HTTP request bodies sent via event triggers (experimental)
# TYPE hasura_event_trigger_request_bytes_total counter
hasura_event_trigger_request_bytes_total 0.0

# HELP hasura_event_trigger_response_bytes_total Total size of HTTP response bodies received via event triggers (experimental)
# TYPE hasura_event_trigger_response_bytes_total counter
hasura_event_trigger_response_bytes_total 0.0

# HELP hasura_events_fetched_per_batch Number of events fetched in a batch
# TYPE hasura_events_fetched_per_batch gauge
hasura_events_fetched_per_batch 0.0

# HELP hasura_graphql_execution_time_seconds Execution time of successful GraphQL requests (excluding subscriptions)
# TYPE hasura_graphql_execution_time_seconds histogram
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="1.0e-2"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="3.0e-2"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="0.1"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="0.3"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="1.0"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="3.0"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="10.0"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="mutation",le="+Inf"} 0
hasura_graphql_execution_time_seconds_sum{operation_type="mutation"} 0.0
hasura_graphql_execution_time_seconds_count{operation_type="mutation"} 0
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="1.0e-2"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="3.0e-2"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="0.1"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="0.3"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="1.0"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="3.0"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="10.0"} 1
hasura_graphql_execution_time_seconds_bucket{operation_type="query",le="+Inf"} 1
hasura_graphql_execution_time_seconds_sum{operation_type="query"} 1.464608e-3
hasura_graphql_execution_time_seconds_count{operation_type="query"} 1

# HELP hasura_graphql_requests_total Number of GraphQL requests received (excluding subscriptions)
# TYPE hasura_graphql_requests_total counter
hasura_graphql_requests_total{response_status="success",operation_type="query",parameterized_query_hash="7116865cef017c3b09e5c9271b0e182a6dcf4c01"} 1.0

# HELP hasura_http_connections Current number of active HTTP connections (excluding WebSocket connections)
# TYPE hasura_http_connections gauge
hasura_http_connections 1.0

# HELP hasura_http_request_bytes_total Total size of HTTP request bodies received via the HTTP server
# TYPE hasura_http_request_bytes_total counter
hasura_http_request_bytes_total 1978.0

# HELP hasura_http_response_bytes_total Total size of HTTP response bodies sent via the HTTP server
# TYPE hasura_http_response_bytes_total counter
hasura_http_response_bytes_total 12154.0

# HELP hasura_metadata_resource_version Current metadata resource version
# TYPE hasura_metadata_resource_version gauge
hasura_metadata_resource_version 307.0

# HELP hasura_oneoff_events_invocation_total Total number of one-off events invoked
# TYPE hasura_oneoff_events_invocation_total counter
hasura_oneoff_events_invocation_total{status="failed"} 0.0
hasura_oneoff_events_invocation_total{status="success"} 0.0

# HELP hasura_oneoff_events_processed_total Total number of one-off events processed
# TYPE hasura_oneoff_events_processed_total counter
hasura_oneoff_events_processed_total{status="failed"} 0.0
hasura_oneoff_events_processed_total{status="success"} 0.0

# HELP hasura_otel_dropped_logs Total number of log lines dropped due to high log volume
# TYPE hasura_otel_dropped_logs counter
hasura_otel_dropped_logs{reason="buffer_full"} 0.0
hasura_otel_dropped_logs{reason="send_failed"} 0.0

# HELP hasura_otel_dropped_spans Total number of trace spans dropped due to high trace volume
# TYPE hasura_otel_dropped_spans counter
hasura_otel_dropped_spans{reason="buffer_full"} 0.0
hasura_otel_dropped_spans{reason="send_failed"} 0.0

# HELP hasura_otel_sent_logs Total number of successfully exported log lines
# TYPE hasura_otel_sent_logs counter
hasura_otel_sent_logs 152.0

# HELP hasura_otel_sent_spans Total number of successfully exported trace spans
# TYPE hasura_otel_sent_spans counter
hasura_otel_sent_spans 25.0

# HELP hasura_scheduled_trigger_request_bytes_total Total size of HTTP request bodies sent via scheduled triggers (experimental)
# TYPE hasura_scheduled_trigger_request_bytes_total counter
hasura_scheduled_trigger_request_bytes_total 0.0

# HELP hasura_scheduled_trigger_response_bytes_total Total size of HTTP response bodies received via scheduled triggers (experimental)
# TYPE hasura_scheduled_trigger_response_bytes_total counter
hasura_scheduled_trigger_response_bytes_total 0.0

# HELP hasura_websocket_connections Current number of active WebSocket connections
# TYPE hasura_websocket_connections gauge
hasura_websocket_connections 0.0

# HELP hasura_websocket_message_queue_time The time (in seconds) for which a websocket message remains queued in the GraphQL engine's websocket queue
# TYPE hasura_websocket_message_queue_time histogram
hasura_websocket_message_queue_time_bucket{le="1.0e-6"} 0
hasura_websocket_message_queue_time_bucket{le="1.0e-4"} 0
hasura_websocket_message_queue_time_bucket{le="1.0e-2"} 0
hasura_websocket_message_queue_time_bucket{le="0.1"} 0
hasura_websocket_message_queue_time_bucket{le="0.3"} 0
hasura_websocket_message_queue_time_bucket{le="1.0"} 0
hasura_websocket_message_queue_time_bucket{le="3.0"} 0
hasura_websocket_message_queue_time_bucket{le="10.0"} 0
hasura_websocket_message_queue_time_bucket{le="30.0"} 0
hasura_websocket_message_queue_time_bucket{le="100.0"} 0
hasura_websocket_message_queue_time_bucket{le="+Inf"} 0
hasura_websocket_message_queue_time_sum 0.0
hasura_websocket_message_queue_time_count 0

# HELP hasura_websocket_message_write_time The time taken (in seconds) to write a websocket message into the TCP send buffer
# TYPE hasura_websocket_message_write_time histogram
hasura_websocket_message_write_time_bucket{le="1.0e-6"} 0
hasura_websocket_message_write_time_bucket{le="1.0e-4"} 0
hasura_websocket_message_write_time_bucket{le="1.0e-2"} 0
hasura_websocket_message_write_time_bucket{le="0.1"} 0
hasura_websocket_message_write_time_bucket{le="0.3"} 0
hasura_websocket_message_write_time_bucket{le="1.0"} 0
hasura_websocket_message_write_time_bucket{le="3.0"} 0
hasura_websocket_message_write_time_bucket{le="10.0"} 0
hasura_websocket_message_write_time_bucket{le="30.0"} 0
hasura_websocket_message_write_time_bucket{le="100.0"} 0
hasura_websocket_message_write_time_bucket{le="+Inf"} 0
hasura_websocket_message_write_time_sum 0.0
hasura_websocket_message_write_time_count 0

# HELP hasura_websocket_messages_received_bytes_total Total size of WebSocket messages received
# TYPE hasura_websocket_messages_received_bytes_total counter
hasura_websocket_messages_received_bytes_total 0.0

```

# Penjelasan Mendetail Metrics Hasura

Berikut adalah penjelasan mendetail mengenai tiap baris dari metrics di atas beserta nilai-nilainya:

## 1. hasura_action_request_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP request bodies yang dikirim melalui action. Ini adalah metrik **eksperimental**.
- **Tipe**: Counter
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada request bodies yang dikirim melalui action hingga saat ini.

## 2. hasura_action_response_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP response bodies yang diterima melalui action. Ini juga metrik **eksperimental**.
- **Tipe**: Counter
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada response bodies yang diterima melalui action.

## 3. hasura_active_subscription_pollers
- **Deskripsi**: Jumlah poller subscription yang aktif saat ini.
- **Tipe**: Gauge
- **Nilai**:
  - `subscription_kind="live-query"`: `0.0`
  - `subscription_kind="streaming"`: `0.0`
- **Penjelasan**: Tidak ada poller subscription aktif untuk kedua jenis subscription (`live-query` dan `streaming`).

## 4. hasura_active_subscription_pollers_in_error_state
- **Deskripsi**: Jumlah poller subscription yang berada dalam status error saat ini.
- **Tipe**: Gauge
- **Nilai**:
  - `subscription_kind="live-query"`: `0.0`
  - `subscription_kind="streaming"`: `0.0`
- **Penjelasan**: Tidak ada poller subscription dalam status error untuk kedua jenis subscription.

## 5. hasura_cache_request_count
- **Deskripsi**: Jumlah total permintaan cache lookup yang diterima.
- **Tipe**: Counter
- **Nilai**:
  - `status="hit"`: `0.0`
  - `status="miss"`: `0.0`
- **Penjelasan**: Tidak ada cache request yang terjadi baik dalam status `hit` maupun `miss`.

## 6. hasura_cron_events_invocation_total
- **Deskripsi**: Jumlah total cron events yang di-invoke (dipanggil).
- **Tipe**: Counter
- **Nilai**:
  - `status="failed"`: `0.0`
  - `status="success"`: `0.0`
- **Penjelasan**: Tidak ada cron event yang berhasil atau gagal dipanggil.

## 7. hasura_cron_events_processed_total
- **Deskripsi**: Jumlah total cron events yang sudah diproses.
- **Tipe**: Counter
- **Nilai**:
  - `status="failed"`: `0.0`
  - `status="success"`: `0.0`
- **Penjelasan**: Tidak ada cron event yang diproses sejauh ini.

## 8. hasura_event_fetch_time_per_batch_seconds
- **Deskripsi**: Latency dalam detik untuk fetching batch event dari database.
- **Tipe**: Histogram
- **Nilai**: Semua bucket bernilai `0`, artinya tidak ada event yang difetch, dengan total waktu `0.0` detik.
- **Penjelasan**: Tidak ada event yang difetch dalam batch dari database.

## 9. hasura_event_trigger_http_workers
- **Deskripsi**: Jumlah event trigger HTTP worker yang aktif saat ini.
- **Tipe**: Gauge
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada HTTP worker event trigger yang aktif.

## 10. hasura_event_trigger_request_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP request bodies yang dikirim melalui event triggers.
- **Tipe**: Counter
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada request bodies yang dikirim melalui event triggers.

## 11. hasura_event_trigger_response_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP response bodies yang diterima melalui event triggers.
- **Tipe**: Counter
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada response bodies yang diterima melalui event triggers.

## 12. hasura_events_fetched_per_batch
- **Deskripsi**: Jumlah event yang difetch dalam satu batch.
- **Tipe**: Gauge
- **Nilai**: `0.0`
- **Penjelasan**: Tidak ada event yang difetch dalam batch.

## 13. hasura_graphql_execution_time_seconds
- **Deskripsi**: Waktu eksekusi untuk GraphQL requests yang berhasil (tidak termasuk subscriptions).
- **Tipe**: Histogram
- **Nilai**:
  - `operation_type="mutation"`: Semua bucket bernilai `0` (tidak ada mutation yang dieksekusi).
  - `operation_type="query"`: Ada 1 query yang dijalankan dengan total waktu `1.464608e-3` detik.
- **Penjelasan**: Hanya ada satu GraphQL query yang dieksekusi dan berhasil dengan waktu eksekusi sangat cepat.

## 14. hasura_graphql_requests_total
- **Deskripsi**: Jumlah total permintaan GraphQL yang diterima (tidak termasuk subscriptions).
- **Tipe**: Counter
- **Nilai**: 1 request yang berhasil dengan operation_type `query`.
- **Penjelasan**: Ada satu permintaan GraphQL yang berhasil dengan operation_type sebagai query.

## 15. hasura_http_connections
- **Deskripsi**: Jumlah HTTP connections yang aktif saat ini (tidak termasuk WebSocket connections).
- **Tipe**: Gauge
- **Nilai**: 1.0
- **Penjelasan**: Ada satu koneksi HTTP aktif.

## 16. hasura_http_request_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP request bodies yang diterima melalui HTTP server.
- **Tipe**: Counter
- **Nilai**: 1978.0
- **Penjelasan**: Ukuran total dari request HTTP yang diterima adalah 1978 bytes.

## 17. hasura_http_response_bytes_total
- **Deskripsi**: Total ukuran (dalam bytes) dari HTTP response bodies yang dikirim melalui HTTP server.
- **Tipe**: Counter
- **Nilai**: 12154.0
- **Penjelasan**: Ukuran total dari response HTTP yang dikirim adalah 12154 bytes.

## 18. hasura_metadata_resource_version
- **Deskripsi**: Versi resource metadata yang sedang aktif.
- **Tipe**: Gauge
- **Nilai**: 307.0
- **Penjelasan**: Versi resource metadata saat ini adalah 307.

## 19. hasura_oneoff_events_invocation_total
- **Deskripsi**: Total jumlah one-off events yang di-invoke.
- **Tipe**: Counter
- **Nilai**:
  - `status="failed"`: `0.0`
  - `status="success"`: `0.0`
- **Penjelasan**: Tidak ada one-off event yang dipanggil atau diproses.

## 20. hasura_oneoff_events_processed_total
- **Deskripsi**: Total jumlah one-off events yang diproses.
- **Tipe**: Counter
- **Nilai**:
  - `status="failed"`: `0.0`
  - `status="success"`: `0.0`
- **Penjelasan**: Tidak ada one-off event yang diproses.

## 21. hasura_otel_dropped_logs
Deskripsi: Jumlah total log yang di-drop karena volume log yang tinggi.
Tipe: Counter
Nilai:
reason="buffer_full": 0.0
reason="send_failed": 0.0
Penjelasan: Tidak ada log yang di-drop karena buffer penuh atau gagal dikirim.

## 22. hasura_otel_dropped_spans
Deskripsi: Jumlah total trace span yang di-drop karena volume trace yang tinggi.
Tipe: Counter
Nilai:
reason="buffer_full": 0.0
reason="send_failed": 0.0
Penjelasan: Tidak ada trace span yang di-drop karena buffer penuh atau gagal dikirim.

## 23. hasura_otel_sent_logs
Deskripsi: Jumlah total log yang berhasil diekspor.
Tipe: Counter
Nilai: 152.0
Penjelasan: Sebanyak 152 log berhasil diekspor ke OpenTelemetry.

## 24. hasura_otel_sent_spans
Deskripsi: Jumlah total trace span yang berhasil diekspor.
Tipe: Counter
Nilai: 3.0
Penjelasan: Sebanyak 3 trace span berhasil diekspor ke OpenTelemetry.

## 25. hasura_pending_events
Deskripsi: Jumlah event yang belum diproses dalam event queue.
Tipe: Gauge
Nilai: 0.0
Penjelasan: Tidak ada event yang menunggu untuk diproses dalam event queue.

## 26. hasura_pending_http_requests
Deskripsi: Jumlah HTTP request yang masih tertunda saat ini.
Tipe: Gauge
Nilai: 0.0
Penjelasan: Tidak ada HTTP request yang tertunda saat ini.

## 27. hasura_schema_sync_last_received_version
Deskripsi: Versi terakhir dari schema metadata yang diterima.
Tipe: Gauge
Nilai: 308.0
Penjelasan: Versi schema metadata terakhir yang diterima adalah 308.

## 28. hasura_schema_sync_last_sent_version
Deskripsi: Versi terakhir dari schema metadata yang dikirim.
Tipe: Gauge
Nilai: 308.0
Penjelasan: Versi schema metadata terakhir yang dikirim adalah 308.

## 29. hasura_schema_sync_queue_size
Deskripsi: Ukuran dari schema sync queue.
Tipe: Gauge
Nilai: 0.0
Penjelasan: Tidak ada item dalam schema sync queue saat ini.

## 30. hasura_websocket_connections
Deskripsi: Jumlah WebSocket connections yang aktif.
Tipe: Gauge
Nilai: 0.0
Penjelasan: Tidak ada WebSocket connection yang aktif saat ini.

## Ringkasan
Sebagian besar nilai dari metrics menunjukkan bahwa sistem dalam keadaan idle atau belum digunakan secara penuh, kecuali pada beberapa metrik seperti `hasura_http_request_bytes_total`, `hasura_http_response_bytes_total`, dan `hasura_otel_sent_logs`, yang menunjukkan adanya aktivitas seperti HTTP request/response dan OpenTelemetry log.
