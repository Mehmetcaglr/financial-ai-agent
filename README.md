readme_content = """# 🤖 Finansal AI Ajanı - ReAct Mimarisi

Bu proje, ReAct (Reasoning and Action) mimarisini kullanarak gerçek zamanlı finansal veri analizi ve yatırım tavsiyesi verebilen bir AI ajanı geliştirir.

## 🎯 Proje Amacı

Kullanıcıların finansal sorularını yanıtlamak için:
- Gerçek zamanlı hisse senedi ve kripto para verilerini çeker
- Finansal kavramları RAG (Retrieval-Augmented Generation) ile açıklar
- Çok adımlı (multi-hop) mantıksal çıkarım yapar
- Yatırım analizi ve risk değerlendirmesi sunar

## 🏗️ Mimari

### ReAct Döngüsü
```
Thought (Düşünce) → Action (Aksiyon) → Observation (Gözlem) → Answer (Cevap)
```

### Bileşenler

1. **LLM Client**: Anthropic Claude veya OpenAI GPT
2. **Tools (Araçlar)**:
   - `get_stock_price`: Hisse senedi fiyatları
   - `get_stock_info`: Şirket bilgileri
   - `get_financial_ratios`: Finansal oranlar
   - `get_currency_rate`: Döviz kurları
   - `get_crypto_price`: Kripto fiyatları
   - `calculator`: Matematiksel hesaplamalar
   - `search_financial_knowledge`: RAG tabanlı bilgi arama

3. **RAG Engine**: Finansal kavramlar için in-memory vector store
4. **Agent**: ReAct döngüsünü yöneten akıllı ajan

## 📊 Veri Kaynakları

- **Yahoo Finance (yfinance)**: Hisse senedi, kripto para ve döviz verileri
- **Bilgi Bankası**: P/E ratio, risk yönetimi, yatırım stratejileri vb.

## 🚀 Kurulum

### Gereksinimler
```bash
pip install -r requirements.txt
```

### API Anahtarları
Google Colab Secrets'a şu anahtarları ekleyin:
- `ANTHROPIC_API_KEY` (önerilen)
- `OPENAI_API_KEY` (opsiyonel)

## 💻 Kullanım

### Google Colab'da Çalıştırma

1. Notebook'u açın: `financial_ai_agent.ipynb`
2. Cell'leri sırayla çalıştırın:
   - Cell 1: Kurulum
   - Cell 2: API Anahtarları
   - Cell 3: LLM Client
   - Cell 4-6: Araçlar ve RAG
   - Cell 7-8: Agent ve ReAct Loop
   - Cell 9: Test
   - Cell 10-13: Benchmark

### Örnek Sorular
```python
# Basit sorgu
run_agent("Tesla'nın güncel fiyatı nedir?")

# Karmaşık analiz
run_agent("NVIDIA hissesi alınır mı? P/E, PEG ve sektör bilgilerini kullanarak analiz yap.")

# Multi-hop reasoning
run_agent("10000 TL'mi %60 Apple, %40 Microsoft'a yatırsam kaç hisse alabilirim?")
```

## 📈 Benchmark Sonuçları

| Zorluk | Başarı Oranı | Ortalama Süre |
|--------|-------------|---------------|
| Kolay | 100% (2/2) | 5.2s |
| Orta | 100% (3/3) | 12.2s |
| Orta-Zor | 100% (1/1) | 14.8s |
| Zor | 100% (2/2) | 25.5s |
| Çok Zor | 100% (2/2) | 24.4s |

**Genel Başarı: %100 (10/10)** ✅

## 🔍 Örnek Test Senaryoları

### Scenario A (One-Shot Query)
**Soru**: "1 Dolar kaç TL?"  
**Beklenen**: Tek API çağrısı ile direkt cevap  
**Sonuç**: ✅ 1 Dolar = 42.93 TL (5.1s)

### Scenario B (Multi-Hop Query)
**Soru**: "Google'ın borç/özsermaye oranı 1'den büyük mü? Bu ne anlama gelir?"  
**Beklenen**: Finansal veri çek → RAG'den açıklama al → Analiz yap  
**Sonuç**: ✅ 11.424 (Yüksek risk) + Detaylı açıklama (14.8s)

## 🛠️ Teknik Detaylar

### RAG Motoru
- **Embedding**: TF-IDF benzeri kelime frekansı
- **Similarity**: Cosine similarity
- **Chunk Size**: 2-3 cümle
- **Top-K**: 3 en ilgili chunk

### Agent Parametreleri
- **Model**: Claude Sonnet 4 / GPT-4
- **Temperature**: 0 (deterministik)
- **Max Turns**: 10-15 (sonsuz döngü önleme)
- **Stop Sequences**: Yok (manuel kontrol)

## 🚧 Karşılaşılan Zorluklar ve Çözümler

### 1. Sonsuz Döngü (Infinite Loop)
**Problem**: Agent aynı action'ı tekrar tekrar çalıştırıyor  
**Çözüm**: `max_turns=10` parametresi eklendi

### 2. Hallucination
**Problem**: Agent olmayan tool'ları çağırmaya çalışıyor  
**Çözüm**: System prompt'ta tool listesi açıkça belirtildi, hata kontrolü eklendi

### 3. Stop Sequence Problemi
**Problem**: LLM yanıtı erken kesiliyor, cevap görünmüyor  
**Çözüm**: Stop sequence'ler kaldırıldı, manuel PAUSE kontrolü eklendi

### 4. Cevap Formatı
**Problem**: Agent "Answer:" yerine farklı formatlar kullanıyor  
**Çözüm**: System prompt'ta örneklerle format vurgulandı



## 🎓 Öğrenilen Kavramlar

1. **ReAct Mimarisi**: Thought-Action-Observation döngüsü
2. **RAG (Retrieval-Augmented Generation)**: Vector store ile bilgi arama
3. **Multi-hop Reasoning**: Çok adımlı mantıksal çıkarım
4. **Tool Calling**: LLM'in dış araçları kullanması
5. **Prompt Engineering**: Etkili system prompt tasarımı

## 🔮 Gelecek İyileştirmeler

- [ ] Daha gelişmiş RAG (ChromaDB/Pinecone entegrasyonu)
- [ ] Gerçek zamanlı haber analizi (web scraping)
- [ ] Teknik analiz göstergeleri (RSI, MACD)
- [ ] Portföy optimizasyonu algoritmaları
- [ ] Multi-agent collaboration (uzman ajanlar)

## 📚 Kaynaklar

- [ReAct Paper](https://arxiv.org/abs/2210.03629)
- [Anthropic Claude API](https://docs.anthropic.com/)
- [Yahoo Finance API](https://pypi.org/project/yfinance/)

## 👨‍💻 Geliştirici

**Mehmet**  
Yazılım Geliştirici | AI/ML Enthusiast  

## 📄 Lisans

Bu proje eğitim amaçlıdır. Yatırım tavsiyesi değildir.

---

**⚠️ Önemli Uyarı**: Bu sistem finansal tavsiye vermez, sadece analiz yapar. Yatırım kararları için profesyonel danışman ile görüşün.
"""

