# Contoh output `stellar contract invoke` (testnet)

Output di bawah diambil dari kontrak checkout di testnet. Field-nya yang penting,
bukan angkanya.

## `order` — record lengkap

```bash
stellar contract invoke --id <CONTRACT_ID> --source-account alice \
  --network testnet -- order --order_id "$ORDER_ID"
```

```json
{
  "buyer": "G...",
  "amount": "10000000",
  "token": "CBIELTK6YBZJU5UP2WWQEUCYKLPU6AUNZ2BQ4WWFEIE3USCIHMXQDAMA",
  "timestamp": "1795155811",
  "status": "Paid"
}
```

`amount` dalam unit mentah. USDC 7 desimal → `10000000` = 1.0 USDC.

## `status` — hanya lifecycle

```bash
stellar contract invoke ... -- status --order_id "$ORDER_ID"
```

```
"Paid"
```

Empat nilai yang mungkin: `Pending`, `Paid`, `Shipped`, `Refunded`.
Hanya `Paid` yang masih di-escrow.

## `merchant` — cek sebelum tanda tangan

```bash
stellar contract invoke ... -- merchant
```

```
"G..."
```

Kalau alamat ini bukan wallet yang akan kamu pakai, **berhenti**. Tanda tangan
akan ditolak, dan transaksi ditolak bukan berarti refund.

## Jebakan `is_paid`

```bash
stellar contract invoke ... -- is_paid --order_id "$ORDER_ID"
```

`is_paid` bernilai `true` untuk **`Paid` dan `Shipped`**. Artinya "dana pernah
masuk", bukan "dana masih ditahan". Pakai `status` kalau perbedaannya penting.
