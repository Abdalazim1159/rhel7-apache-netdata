# Monitoring Apache on RHEL 7 with Netdata + ApacheBench

> A Production-like Lab: Technical Support Website + Load Testing + Real-time Monitoring

### 📦 Stack
- **OS:** RHEL 7.9
- **Web Server:** Apache httpd 2.4.6
- **Monitoring:** Netdata Agent (Offline)
- **Load Test:** ApacheBench (ab)

---

### 1. Install Apache
```bash
sudo yum install httpd -y
sudo systemctl enable --now httpd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo mkdir -p /var/www/html/support
sudo systemctl restart httpd

##netdata install
اللهم

# Move and install
/tmp/netdata-*.gz.run /tmp/
sudo chmod +x /tmp/netdata-*.gz.run
sudo /tmp/netdata-*.gz.run --accept

# Open port & restart
sudo firewall-cmd --add-port=19999/tcp --permanent
sudo firewall-cmd --reload
sudo setenforce 0
sudo systemctl restart netdata

# Fix Web Log permissions
sudo chmod 755 /var/log/httpd
sudo chmod 644 /var/log/httpd/access_log
sudo setfacl -m u:netdata:r /var/log/httpd/access_log
sudo systemctl restart netdata

3. Load Test
sudo yum install httpd-tools -y
ab -n 100 -c 50 http://192.168.91.129/support/
Result: 465 req/sec, 0 Failed  
Spike: 1.0 -> 16.5 req/s in Netdata is the exact moment of the ApacheBench test.

4. Verify in Netdata
- Web Log -> Requests/sec
- Apache -> Connections
- System Overview -> CPU / RAM


Built by System Administrator:
Abdaleazim kamal 
