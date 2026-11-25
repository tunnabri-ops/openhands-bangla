# OpenHands / Agent Canvas বাংলা সেটআপ গাইড

OpenHands এখন **Agent Canvas** নামে চলে — AI এজেন্ট আপনার হয়ে কোড লেখে, ফাইল
বানায়, টেস্ট চালায়। Original repo: https://github.com/OpenHands/OpenHands

## ১. যা যা লাগবে

- Docker (Docker Desktop / Docker Engine) — চালু থাকতে হবে
- একটি LLM API key — OpenAI, Anthropic, OpenRouter, বা লোকাল মডেল
- আপনার প্রজেক্টের জন্য একটা ফোল্ডার

## ২. Agent Canvas চালু করা (Docker)

```bash
# ১. প্রজেক্ট ফোল্ডার বানান (আপনার প্রজেক্টগুলো এখানে থাকবে)
mkdir -p ~/projects

# ২. Agent Canvas চালু করুন
docker run -it --rm \
  -p 127.0.0.1:8000:8000 \
  -e AGENT_CANVAS_ALLOW_LAN_SESSION_KEY=true \
  -v openhands-state:/home/openhands/.openhands \
  -v "$HOME/projects:/projects" \
  ghcr.io/openhands/agent-canvas:1.45.0
```

> ইমেজের নাম upstream README থেকে নেওয়া ও GHCR API দিয়ে যাচাই করা (manifest HTTP 200)।
> পুরোনো `docker.all-hands.dev/all-hands-ai/openhands` ইমেজ আর বর্তমান নয় — ওটা ব্যবহার করবেন না।

ব্রাউজারে খুলুন: **http://localhost:8000/canvas**

সেটিংস, গোপন তথ্য ও কথোপকথন `openhands-state` ভলিউমে থাকে, তাই কন্টেইনার
বন্ধ করলেও ডেটা হারায় না। আবার চালু করলে একই ভলিউম ব্যবহার করুন।

**Linux Docker Engine-এ একটা কথা:** কন্টেইনারের ভেতরের uid 10001-এর
`~/projects` ফোল্ডার পড়া-লেখার অনুমতি থাকতে হবে। অনুমতি না থাকলে:

```bash
find ~/projects -type d -exec chmod a+rwX {} +
find ~/projects -type f -exec chmod a+rw {} +
```

## ৩. LLM সংযোগ (V1 — এটাই বর্তমান পদ্ধতি)

V1-এ সেটিংস মূলত **Settings UI** ও **environment variable** দিয়ে করা হয়:

1. ব্রাউজারে http://localhost:8000/canvas খুলুন
2. **Settings** > **LLM**-এ যান
3. প্রোভাইডার বেছে API key ও মডেল দিন (যেমন OpenAI `gpt-4o`, বা OpenRouter-এর মডেল)

অথবা Docker চালানোর সময় environment variable দিয়ে:

```bash
docker run -it --rm \
  -p 127.0.0.1:8000:8000 \
  -e AGENT_CANVAS_ALLOW_LAN_SESSION_KEY=true \
  -e LLM_API_KEY="sk-আপনার-key" \
  -e LLM_MODEL="gpt-4o" \
  -v openhands-state:/home/openhands/.openhands \
  -v "$HOME/projects:/projects" \
  ghcr.io/openhands/agent-canvas:1.45.0
```

> আসল API key কখনো গিটে কমিট করবেন না, কারো সাথে শেয়ার করবেন না।

## ৪. config.toml কখন ব্যবহার করবেন?

`config.toml` হলো **V0 (legacy)** ফরম্যাট — পুরোনো OpenHands CLI/Headless
চালাতে বা রেফারেন্স হিসেবে কাজে লাগে। V1 Agent Canvas-এ এটা দরকার নেই;
তবে ফাইলটি এই রেপোতে রাখা আছে, সব কি upstream `config.template.toml`
(ট্যাগ `0.62.0`) থেকে নেওয়া। V0 ব্যবহার করলে ফাইলটি `~/.openhands/config.toml`
হিসেবে রাখতে হয় — বিস্তারিত: https://docs.openhands.dev/openhands/usage/configuration-options

## ৫. প্রথম কাজ

`first-task-bangla.md` খুলে যেকোনো একটা প্রম্পট কপি করে চ্যাটে পেস্ট করুন।
এজেন্ট কাজ শুরু করবে — প্রতিটা ধাপে কী করছে, তা চ্যাটেই দেখাবে।

## ৬. সমস্যা হলে

| সমস্যা | সমাধান |
|---|---|
| `localhost:8000` খুলছে না | `docker ps` দিয়ে দেখুন কন্টেইনার চলছে কি না; লগ দেখুন `docker logs <container>` |
| `Permission denied` (Linux) | উপরের ধাপ ২-এর `chmod` কমান্ড চালান |
| LLM "not connected" | Settings-এ API key ও মডেল ঠিক আছে কি না দেখুন; key-এর ব্যালেন্স/কোটা দেখুন |
| এজেন্ট ভুল ফাইলে হাত দিল | পরের বার প্রম্পটে লিখুন: "আগে জানাও, অনুমতি ছাড়া কিছু বদলিও না" |

বিস্তারিত ডক্স: https://docs.openhands.dev/
প্রথমবারের সেটআপ: https://docs.openhands.dev/openhands/usage/agent-canvas/first-time-setup
