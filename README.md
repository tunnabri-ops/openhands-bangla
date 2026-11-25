# OpenHands Bangla — AI দিয়ে কোড লেখা, Agent Canvas দিয়ে

OpenHands (এখন Agent Canvas) Docker সেটআপ + কনফিগ টেমপ্লেট + প্রথম কাজের প্রম্পট।

- **Docs — Configuration:** https://docs.openhands.dev/openhands/usage/configuration-options
- **Docs — First Time Setup:** https://docs.openhands.dev/openhands/usage/agent-canvas/first-time-setup

## কী আছে / What is inside

- `config.toml` — OpenHands সেটিংস টেমপ্লেট (অফিসিয়াল `config.template.toml`-এর আসল কি-গুলোই শুধু: core/llm/agent/sandbox/security/condenser)।
- `first-task-bangla.md` — প্রথম কোডিং কাজের জন্য বাংলায় তৈরি প্রম্পট (৩টি কাজ, কপি-পেস্ট করলেই চলে)।
- `setup-bangla.md` — Docker দিয়ে Agent Canvas চালানোর বাংলা গাইড।

## যা ঠিক করা হয়েছে / What was fixed

- **গুরুত্বপূর্ণ:** OpenHands এখন **Agent Canvas** নামে চলে — পুরোনো `docker.all-hands.dev/all-hands-ai/openhands` ইমেজ আর ডিফল্ট নয়। বর্তমান ইমেজ `ghcr.io/openhands/agent-canvas:1.45.0` — GHCR API দিয়ে যাচাই করা (manifest HTTP 200)।
- `config.toml` হলো V0 (legacy) ফরম্যাট — V1-এ বেশিরভাগ সেটিং এখন Settings UI ও environment variable-এ করা হয় (`LLM_API_KEY`, `LLM_MODEL` — ডক্স থেকে যাচাই করা)। দুটো পথই এই রেপোতে দেওয়া আছে, কোনটা কখন ব্যবহার করবেন তা `setup-bangla.md`-এ লেখা।
- config-এর কোনো কি আন্দাজে লেখা হয়নি — সব কি অফিসিয়াল `config.template.toml` (ট্যাগ `0.62.0`) থেকে নেওয়া।

## ব্যবহার / How to use

```bash
# 1. প্রজেক্ট ফোল্ডার বানান
mkdir -p ~/projects

# 2. Agent Canvas চালু করুন (এই ফোল্ডার থেকে)
docker run -it --rm \
  -p 127.0.0.1:8000:8000 \
  -e AGENT_CANVAS_ALLOW_LAN_SESSION_KEY=true \
  -v openhands-state:/home/openhands/.openhands \
  -v "$HOME/projects:/projects" \
  ghcr.io/openhands/agent-canvas:1.45.0

# 3. ব্রাউজারে খুলুন: http://localhost:8000/canvas
```

তারপর Settings-এ LLM API key ও মডেল দিন, `first-task-bangla.md` থেকে প্রথম কাজের প্রম্পট কপি করে চ্যাটে পেস্ট করুন।
