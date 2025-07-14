# WAD-MCA v1.0: Web Attack Dataset with Multi-Class Labels and Payload Annotations  
🔗 **GitHub Repository**: `https://github.com/bouz34/WAD-MCA`  

## 📊 Dataset Distribution
| **Attack Type**      | **Number of Samples** | **Proportion (%)** |  
|----------------------|----------------------|--------------------|  
| Command Injection    | 1,900                | 17.9               |  
| Path Traversal       | 2,096                | 19.7               |  
| SQL Injection        | 1,839                | 17.3               |  
| XSS                  | 2,648                | 24.9               |  
| Normal               | 2,139                | 20.1               |  
| **Total**            | **10,622**           | **100.0**          |  

## 📌 Dataset Overview  
WAD-MCA v1.0 is a **multi-class labeled web attack dataset** rigorously constructed from 5 authoritative sources. It provides:  
- ✅ **10,622 precisely annotated samples** (8,483 attacks + 2,139 normal)  
- ✅ **Fine-grained payload isolation** with exact boundary markers  
- ✅ **Natural non-balanced distribution** reflecting real-world attack patterns  
- ✅ **Enhanced path diversity** with 9,127 unique URL paths  

## 🔧 Construction Methodology  
### Integrated Source Datasets  
| **Source** | **Contribution** | **License** |  
|------------|------------------|-------------|  
| [HTTP CSIC 2010](http://www.isi.csic.es/dataset/) | Base attack traffic with URL payloads | [CC BY-NC-SA 3.0](https://creativecommons.org/licenses/by-nc-sa/3.0/) |  
| [MACCDC 2012 http.log](http://www.secrepo.com/) | 8,483 non-redundant paths & contexts | [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) |  
| [payloads-master](https://github.com/foospidy/payloads) | Attack variants for payload substitution | *Unknown (Use with caution)* |  
| [Malicious URL Dataset](https://github.com/faizann24/Using-machine-learning-to-detect-malicious-URLs) | Normal URL expansion | [MIT License](https://github.com/faizann24/Using-machine-learning-to-detect-malicious-URLs/blob/master/LICENSE) |  
| [WebAttack-CVSSMetrics](https://huggingface.co/datasets/chYassine/WebAttack-CVSSMetrics) | SQLi/CMDi prototype samples | [CC BY 4.0](https://huggingface.co/datasets/chYassine/WebAttack-CVSSMetrics/blob/main/README.md) |  

### Key Technical Operations  
1. **Payload Isolation**  
   Extracted exact attack fragments from URL parameters with boundary indices (e.g., `?id=<script>alert(1)</script>` → payload: `"<script>alert(1)</script>"` at index 8)  
   
2. **Hybrid Annotation**  
   LLM-assisted pre-tagging + expert validation for 5 classes:  
   `Command Injection` | `Path Traversal` | `SQL Injection` | `XSS` | `Normal`  
   
3. **Data Recombination**  
   Generated new samples via random pairing of MACCDC 2012 paths and payloads-master variants  

## 📂 Data Format (JSON)  
```json
{
  "url": "/download.php?file=../../etc/passwd",
  "attack_type": "Path Traversal",
  "payload": "../../etc/passwd"
},
{
  "url": "/checkout.php?user=<script>alert(1)</script>",
  "attack_type": "XSS",
  "payload": "<script>alert(1)</script>"
}
```  

### Field Definitions  
| **Field** | **Description** | **Example** |  
|-----------|-----------------|-------------|  
| `url` | Full URL with parameters | `/search?q=<script>alert()</script>` |  
| `attack_type` | One of 5 classes (exact match to Table I) | `"Path Traversal"` |  
| `payload` | Malicious substring in parameters | `"../../etc/passwd"` |  

> ⚠️ **Note**: Normal samples have `"attack_type": "Normal"` and `"payload": null`  


## ⚖️ License Compliance  
This dataset adheres to **source license inheritance**:  
- All derivative works comply with original licenses  
- `payloads-master` entries are flagged in metadata for compliance review  
- Commercial users must conduct independent legal assessment  


---  
💡 **Verification Tip**: Run `validate_dataset.py` to confirm distribution matches Table I  
🐛 Report issues: [GitHub Issues](https://github.com/bouz34/WAD-MCA/issues)
