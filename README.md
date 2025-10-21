# Otomasi-Jaringan---nslookup
Berisi program yang terkait dengan nslookup

# Program ini digunakan untuk menampilkan :
✅ A Record (IP)
✅ MX Record (Mail Exchange)
✅ NS Record (Name Server)
✅ RTT (Round Trip Time)
✅ Status DNS Resolve

# Menyimpan file dengan nama nano dns_lookup_plus.py

# Mengetik program seperti dibawah ini
#!/usr/bin/env python3
# ============================================================
# 🌐 DNS Lookup Plus (DevASC Edition)
# ============================================================
# Fitur:
# - Ambil A record (alamat IP)
# - Ambil MX record (mail exchanger)
# - Ambil NS record (name server)
# - Hitung RTT (round-trip time)
# - Menampilkan status resolusi DNS
# ============================================================

import subprocess
import re

def run_command(cmd):
    """Menjalankan perintah shell dan mengembalikan output-nya."""
    try:
        result = subprocess.check_output(cmd, stderr=subprocess.STDOUT, text=True)
        return result
    except subprocess.CalledProcessError as e:
        return e.output

def get_a_record(domain):
    """Ambil alamat IP (A record)"""
    cmd = ["nslookup", domain]
    output = run_command(cmd)
    ips = re.findall(r"Address:\s+([\d\.]+)", output)
    return ips, output

def get_mx_record(domain):
    """Ambil MX record"""
    cmd = ["nslookup", "-type=mx", domain]
    output = run_command(cmd)
    mx = re.findall(r"mail exchanger = (.+)", output)
    return mx, output

def get_ns_record(domain):
    """Ambil NS record"""
    cmd = ["nslookup", "-type=ns", domain]
    output = run_command(cmd)
    ns = re.findall(r"nameserver = (.+)", output)
    return ns, output

def get_rtt(domain):
    """Hitung RTT rata-rata menggunakan ping"""
    cmd = ["ping", "-c", "4", domain]
    output = run_command(cmd)
    match = re.search(r"= [\d\.]+/([\d\.]+)/", output)
    if match:
        return match.group(1) + " ms"
    else:
        return "N/A"

def main():
    print("=====================================")
    print("     🌍 DNS Lookup Plus (DevASC)     ")
    print("=====================================")
    domain = input("Masukkan nama domain (contoh: google.com): ").strip()

    if not domain:
        print("❌ Domain tidak boleh kosong!")
        return

    print("\n🔎 Mengecek A Record (IP Address)...")
    a_records, a_out = get_a_record(domain)
    if a_records:
        print("✅ Alamat IP ditemukan:")
        for ip in a_records:
            print(f"   - {ip}")
    else:
        print("❌ Tidak ada A record ditemukan.")
    
    print("\n📬 Mengecek MX Record (Mail Exchanger)...")
    mx_records, _ = get_mx_record(domain)
    if mx_records:
        for mx in mx_records:
            print(f"   - {mx}")
    else:
        print("❌ Tidak ada MX record ditemukan.")
    
    print("\n🧭 Mengecek NS Record (Name Server)...")
    ns_records, _ = get_ns_record(domain)
    if ns_records:
        for ns in ns_records:
            print(f"   - {ns}")
    else:
        print("❌ Tidak ada NS record ditemukan.")

    print("\n📡 Mengukur RTT (ping 4x)...")
    rtt = get_rtt(domain)
    print(f"   - Rata-rata RTT: {rtt}")

    print("\n📄 Status Akhir:")
    print(f"   Domain : {domain}")
    print(f"   IP      : {', '.join(a_records) if a_records else 'N/A'}")
    print(f"   MX      : {', '.join(mx_records) if mx_records else 'N/A'}")
    print(f"   NS      : {', '.join(ns_records) if ns_records else 'N/A'}")
    print(f"   RTT     : {rtt}")
    print("   Status  : OK ✅" if a_records else "   Status  : FAILED ❌")
    print("=====================================")

if __name__ == "__main__":
    main()

# Menyimpan kode program dengan cara ctrl o, enter, crtl x

# Menjalankan program dengan mengetik perintah python3 dns_lookup_plus.py

# Kemungkinan output nya akan tampak seperti dibawah ini
=====================================
     🌍 DNS Lookup Plus (DevASC)     
=====================================
Masukkan nama domain (contoh: google.com): google.com

🔎 Mengecek A Record (IP Address)...
✅ Alamat IP ditemukan:
   - 127.0.0.53
   - 64.233.170.102
   - 64.233.170.100
   - 64.233.170.101
   - 64.233.170.139
   - 64.233.170.138
   - 64.233.170.113
   - 2404
   - 2404
   - 2404
   - 2404

📬 Mengecek MX Record (Mail Exchanger)...
   - 10 smtp.google.com.

🧭 Mengecek NS Record (Name Server)...
   - ns1.google.com.
   - ns3.google.com.
   - ns4.google.com.
   - ns2.google.com.

📡 Mengukur RTT (ping 4x)...
   - Rata-rata RTT: 23.113 ms

📄 Status Akhir:
   Domain : google.com
   IP      : 127.0.0.53, 64.233.170.102, 64.233.170.100, 64.233.170.101, 64.233.170.139, 64.233.170.138, 64.233.170.113, 2404, 2404, 2404, 2404
   MX      : 10 smtp.google.com.
   NS      : ns1.google.com., ns3.google.com., ns4.google.com., ns2.google.com.
   RTT     : 23.113 ms
   Status  : OK ✅
=====================================
