import * as React from "react";
import { motion, useScroll, useTransform } from "framer-motion";
import { Instagram, Mail, Phone, Camera, Scissors, Bot, Send, Sparkles } from "lucide-react";

const Button = ({
  children,
  className = "",
  ...props
}: React.ButtonHTMLAttributes<HTMLButtonElement>) => {
  return (
    <button className={className} {...props}>
      {children}
    </button>
  );
};

const heroPhoto = "https://i.postimg.cc/QC4v8XsJ/Full-Size-Render.jpg";

const portfolioPhotos = [
  "https://i.postimg.cc/MpFZPSQt/8C8BB12F-C821-4A87-AD80-0E882496D602.jpg",
  "https://i.postimg.cc/Prnwymjr/DSC00049.jpg",
  heroPhoto,
];

const services = [
  {
    title: "Photography",
    text: "Fashion shoots, portraits and visual direction.",
    icon: Camera,
    number: "01",
  },
  {
    title: "Barber",
    text: "Modern cuts, clean detail and personal style.",
    icon: Scissors,
    number: "02",
  },
  {
    title: "Model",
    text: "Creative collaborations and image work.",
    icon: Instagram,
    number: "03",
  },
];

const quickPrompts = [
  "Who is Rebar?",
  "How can I contact you?",
  "What do you do?",
  "Do you do collaborations?",
];

type Message = {
  role: "ai" | "user";
  text: string;
};

type RevealProps = {
  children: React.ReactNode;
  delay?: number;
  y?: number;
  x?: number;
  scale?: number;
  className?: string;
};

function Reveal({
  children,
  delay = 0,
  y = 30,
  x = 0,
  scale = 0.985,
  className = "",
}: RevealProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y, x, scale, filter: "blur(10px)" }}
      whileInView={{ opacity: 1, y: 0, x: 0, scale: 1, filter: "blur(0px)" }}
      viewport={{ once: true, amount: 0.22 }}
      transition={{ duration: 1.05, delay, ease: [0.22, 1, 0.36, 1] }}
      className={className}
    >
      {children}
    </motion.div>
  );
}

function MarqueeLine({ text, reverse = false }: { text: string; reverse?: boolean }) {
  return (
    <div className="overflow-hidden border-y border-white/8 py-4">
      <motion.div
        animate={{ x: reverse ? ["-18%", "0%"] : ["0%", "-18%"] }}
        transition={{ duration: 20, repeat: Infinity, ease: "linear" }}
        className="flex whitespace-nowrap text-[10px] uppercase tracking-[0.45em] text-white/38 md:text-xs"
      >
        {Array.from({ length: 10 }).map((_, i) => (
          <span key={i} className="mr-10">
            {text}
          </span>
        ))}
      </motion.div>
    </div>
  );
}

export default function RZPersonalSite() {
  const [heroError, setHeroError] = React.useState(false);
  const [chatOpen, setChatOpen] = React.useState(false);
  const [chatInput, setChatInput] = React.useState("");
  const [isTyping, setIsTyping] = React.useState(false);
  const [messages, setMessages] = React.useState<Message[]>([
    {
      role: "ai",
      text: "Hi, I’m the RZ AI assistant. I can tell people about Rebar, his work, collaborations and how to contact him.",
    },
  ]);

  const buildReply = (rawInput: string) => {
    const text = rawInput.toLowerCase().trim();

    if (!text) return "Write a message and I’ll help you with information about Rebar.";
    if (text.includes("who") || text.includes("rebar") || text.includes("rz")) {
      return "Rebar, also known as RZ, is a model, barber and photographer. This site is his personal portfolio and contact page.";
    }
    if (
      text.includes("contact") ||
      text.includes("email") ||
      text.includes("phone") ||
      text.includes("instagram")
    ) {
      return "You can contact Rebar on Instagram @rebozee, by email at rebar.zebaree@gmail.com, or by phone at +39 350 196 4857.";
    }
    if (
      text.includes("what do you do") ||
      text.includes("services") ||
      text.includes("work") ||
      text.includes("job")
    ) {
      return "Rebar works in three areas: modeling, barbering and photography. He is available for creative work, visual projects and personal branding collaborations.";
    }
    if (
      text.includes("collab") ||
      text.includes("booking") ||
      text.includes("book") ||
      text.includes("available")
    ) {
      return "Yes, Rebar is open to collaborations. The best way is to send a message on Instagram or email with your idea, date and project details.";
    }
    if (text.includes("photo") || text.includes("photography")) {
      return "Rebar works on fashion shoots, portraits and visual direction. You can use the portfolio section to see the style and contact him for projects.";
    }
    if (text.includes("barber") || text.includes("hair") || text.includes("cut")) {
      return "Rebar also works as a barber, focused on modern cuts, clean detail and personal style.";
    }
    if (text.includes("model") || text.includes("fashion")) {
      return "Rebar also works as a model for fashion, personal image and collaborations.";
    }
    return "I can help with information about Rebar, his work, collaborations and contact details. Ask me anything.";
  };

  const sendMessage = (forcedText?: string) => {
    const value = (forcedText ?? chatInput).trim();
    if (!value || isTyping) return;

    setMessages((prev) => [...prev, { role: "user", text: value }]);
    setChatInput("");
    setIsTyping(true);

    window.setTimeout(() => {
      setMessages((prev) => [...prev, { role: "ai", text: buildReply(value) }]);
      setIsTyping(false);
    }, 650);
  };

  const { scrollYProgress } = useScroll();
  const progressWidth = useTransform(scrollYProgress, [0, 1], ["0%", "100%"]);
  const heroImageScale = useTransform(scrollYProgress, [0, 1], [1, 1.12]);
  const heroImageY = useTransform(scrollYProgress, [0, 1], [0, -70]);
  const heroTextY = useTransform(scrollYProgress, [0, 1], [0, -110]);
  const heroOverlayOpacity = useTransform(scrollYProgress, [0, 0.4], [0.34, 0.54]);

  return (
    <div className="min-h-screen overflow-x-hidden bg-[#050505] text-[#f5efe6] selection:bg-[#e7c49a] selection:text-black">
      <motion.div
        style={{ width: progressWidth }}
        className="fixed left-0 top-0 z-[100] h-[2px] bg-[#e7c49a] shadow-[0_0_20px_rgba(231,196,154,0.65)]"
      />

      <button
        type="button"
        onClick={() => setChatOpen((prev) => !prev)}
        className="fixed bottom-6 right-6 z-[120] flex items-center gap-2 rounded-full border border-white/10 bg-[#f5efe6] px-5 py-3 text-black shadow-[0_10px_30px_rgba(0,0,0,0.35)] transition duration-300 hover:scale-[1.02]"
      >
        <Bot size={18} />
        AI
      </button>

      {chatOpen && (
        <div className="fixed bottom-24 right-6 z-[120] w-[340px] rounded-[1.6rem] border border-white/10 bg-[#0e0e10]/95 p-4 shadow-[0_20px_60px_rgba(0,0,0,0.45)] backdrop-blur-2xl">
          <div className="mb-3 flex items-center gap-2 text-sm font-medium text-white">
            <Sparkles size={15} className="text-[#e7c49a]" />
            RZ AI Assistant
          </div>

          <div className="mb-3 flex flex-wrap gap-2">
            {quickPrompts.map((prompt) => (
              <button
                key={prompt}
                type="button"
                onClick={() => sendMessage(prompt)}
                className="rounded-full border border-white/10 bg-white/[0.05] px-3 py-1 text-[11px] uppercase tracking-[0.12em] text-white/72 transition hover:bg-white/[0.08]"
              >
                {prompt}
              </button>
            ))}
          </div>

          <div className="mb-3 max-h-72 space-y-2 overflow-y-auto pr-1 text-sm">
            {messages.map((message, index) => (
              <div
                key={`${message.role}-${index}`}
                className={`rounded-2xl px-4 py-3 leading-6 ${
                  message.role === "ai"
                    ? "bg-white/[0.05] text-white/86"
                    : "ml-8 bg-[#e7c49a] text-black"
                }`}
              >
                {message.text}
              </div>
            ))}

            {isTyping && (
              <div className="rounded-2xl bg-white/[0.05] px-4 py-3 text-white/60">
                AI is typing...
              </div>
            )}
          </div>

          <div className="flex gap-2">
            <input
              value={chatInput}
              onChange={(e) => setChatInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") sendMessage();
              }}
              placeholder="Ask anything about Rebar..."
              className="flex-1 rounded-xl border border-white/10 bg-white/[0.04] px-3 py-2 text-sm text-white outline-none placeholder:text-white/35"
            />
            <button
              type="button"
              onClick={() => sendMessage()}
              className="rounded-xl bg-[#f5efe6] px-3 text-black transition hover:bg-[#e7c49a]"
            >
              <Send size={16} />
            </button>
          </div>
        </div>
      )}

      <div className="fixed inset-0 -z-30 bg-[radial-gradient(circle_at_top,rgba(231,196,154,0.14),transparent_24%),linear-gradient(to_bottom,#050505,#0a0a0a,#050505)]" />
      <div className="fixed inset-0 -z-20 opacity-[0.045] [background-image:linear-gradient(to_right,white_1px,transparent_1px),linear-gradient(to_bottom,white_1px,transparent_1px)] [background-size:64px_64px]" />

      <section className="relative flex min-h-screen items-center overflow-hidden px-6 py-10 md:px-10">
        <motion.div style={{ y: heroImageY, scale: heroImageScale }} className="absolute inset-0">
          {!heroError ? (
            <motion.img
              src={heroPhoto}
              alt="Rebar background"
              onError={() => setHeroError(true)}
              className="absolute inset-0 h-full w-full object-cover opacity-60"
            />
          ) : (
            <div className="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(231,196,154,0.14),transparent_24%),linear-gradient(to_bottom,#050505,#0a0a0a,#050505)]" />
          )}

          <motion.div style={{ opacity: heroOverlayOpacity }} className="absolute inset-0 bg-black" />
          <div className="absolute inset-0 bg-[linear-gradient(to_bottom,rgba(5,5,5,0.18),rgba(5,5,5,0.5),rgba(5,5,5,0.95))]" />
          <div className="absolute inset-0 bg-[radial-gradient(circle_at_center,transparent_18%,rgba(0,0,0,0.45)_100%)]" />
        </motion.div>

        <motion.div
          style={{ y: heroTextY }}
          className="relative z-10 mx-auto flex min-h-[60vh] w-full max-w-7xl flex-col items-center justify-center text-center"
        >
          <Reveal y={36} scale={0.96}>
            <h1 className="text-[5rem] font-semibold leading-[0.9] tracking-[-0.03em] text-white drop-shadow-[0_10px_40px_rgba(0,0,0,0.55)] md:text-[9rem] lg:text-[12rem]">
              RZ
            </h1>
          </Reveal>

          <Reveal delay={0.16} y={20}>
            <p className="mt-6 text-lg tracking-[0.2em] text-white/70 md:text-xl">
              Model · Barber · Photographer
            </p>
          </Reveal>
        </motion.div>
      </section>

      <section className="pb-10">
        <MarqueeLine text="RZ — REBAR — MODEL — BARBER — PHOTOGRAPHER" />
        <MarqueeLine text="PORTFOLIO — CONTACT — VISUAL IDENTITY — PERSONAL BRAND" reverse />
      </section>

      <section className="relative mx-auto max-w-7xl px-6 py-24 md:px-10">
        <div className="grid gap-12 lg:grid-cols-[0.8fr_1.2fr] lg:items-start">
          <div className="lg:sticky lg:top-24">
            <Reveal x={-36}>
              <div className="text-[11px] uppercase tracking-[0.35em] text-white/46">About</div>
              <h2 className="mt-5 text-5xl font-semibold leading-[0.92] text-white md:text-7xl">
                About
                <span className="block text-white/60">Me.</span>
              </h2>
            </Reveal>
          </div>

          <div className="space-y-6">
            <Reveal x={42} y={34}>
              <div className="rounded-[2rem] border border-white/10 bg-white/[0.04] p-8 backdrop-blur-2xl shadow-[0_10px_40px_rgba(0,0,0,0.22)] md:p-10">
                <p className="text-xl leading-[1.7] text-white/76 md:text-2xl">
                  My name is <span className="text-white">Rebar</span>, also known as <span className="text-white">RZ</span>. I work across image, style and personal presence.
                </p>
              </div>
            </Reveal>

            <Reveal delay={0.1} x={42} y={28}>
              <div className="rounded-[2rem] border border-white/10 bg-white/[0.03] p-8 backdrop-blur-2xl shadow-[0_10px_40px_rgba(0,0,0,0.18)] md:p-10">
                <p className="text-base leading-8 text-white/60 md:text-lg">
                  This layout is built to feel more editorial and high-end: large typography, controlled motion, stronger spacing and sections that reveal with more confidence while you scroll.
                </p>
              </div>
            </Reveal>
          </div>
        </div>
      </section>

      <section className="mx-auto max-w-7xl px-6 pb-24 md:px-10">
        <Reveal className="mb-10" x={-24}>
          <div className="text-[11px] uppercase tracking-[0.35em] text-white/46">What I do</div>
        </Reveal>

        <div className="space-y-4">
          {services.map((service, index) => {
            const Icon = service.icon;
            return (
              <motion.div
                key={service.title}
                initial={{ opacity: 0, y: 34, scale: 0.985, filter: "blur(10px)" }}
                whileInView={{ opacity: 1, y: 0, scale: 1, filter: "blur(0px)" }}
                viewport={{ once: true, amount: 0.18 }}
                transition={{ duration: 1, delay: index * 0.08, ease: [0.22, 1, 0.36, 1] }}
                className="group rounded-[2rem] border border-white/10 bg-white/[0.03] p-6 backdrop-blur-2xl transition duration-700 hover:-translate-y-1 hover:bg-white/[0.05] md:p-8"
              >
                <div className="grid items-center gap-6 md:grid-cols-[90px_1fr_auto]">
                  <div className="text-4xl font-semibold text-white/28 md:text-5xl">{service.number}</div>
                  <div>
                    <div className="flex items-center gap-4">
                      <Icon className="text-[#e7c49a]" size={24} />
                      <h3 className="text-2xl font-medium text-white md:text-3xl">{service.title}</h3>
                    </div>
                    <p className="mt-4 max-w-2xl leading-8 text-white/60">{service.text}</p>
                  </div>
                  <div className="hidden text-sm uppercase tracking-[0.28em] text-white/30 md:block">RZ</div>
                </div>
              </motion.div>
            );
          })}
        </div>
      </section>

      <section className="mx-auto max-w-7xl px-6 pb-24 md:px-10">
        <Reveal className="mb-10" x={-24}>
          <div className="text-[11px] uppercase tracking-[0.35em] text-white/46">Portfolio</div>
          <h2 className="mt-5 text-5xl font-semibold leading-[0.92] text-white md:text-7xl">
            Visual mood.
          </h2>
        </Reveal>

        <div className="grid gap-6 md:grid-cols-3">
          {portfolioPhotos.map((photo, index) => (
            <motion.div
              key={`${photo}-${index}`}
              initial={{ opacity: 0, y: 34, scale: 0.985, filter: "blur(10px)" }}
              whileInView={{ opacity: 1, y: 0, scale: 1, filter: "blur(0px)" }}
              viewport={{ once: true, amount: 0.18 }}
              transition={{ duration: 1, delay: index * 0.08, ease: [0.22, 1, 0.36, 1] }}
              className={`${index === 2 ? "md:col-span-2" : ""} overflow-hidden rounded-[2rem] border border-white/10 bg-white/[0.03] shadow-[0_10px_40px_rgba(0,0,0,0.24)]`}
            >
              <motion.img
                whileHover={{ scale: 1.05, y: -4 }}
                transition={{ duration: 1.15, ease: [0.22, 1, 0.36, 1] }}
                src={photo}
                alt={`Portfolio ${index + 1}`}
                className={`${index === 2 ? "h-[520px]" : "h-[420px]"} w-full object-cover`}
              />
            </motion.div>
          ))}
        </div>
      </section>

      <section className="mx-auto max-w-5xl px-6 pb-32 text-center md:px-10">
        <Reveal>
          <div className="text-[11px] uppercase tracking-[0.35em] text-white/46">Contact</div>
          <h2 className="mt-5 text-5xl font-semibold leading-[0.92] text-white md:text-7xl">
            Let’s connect.
          </h2>
        </Reveal>

        <Reveal delay={0.08} className="mt-12" y={26}>
          <div className="space-y-5 text-white/80">
            <div className="flex items-center justify-center gap-3">
              <Instagram size={18} className="text-[#e7c49a]" />
              <span>@rebozee</span>
            </div>
            <div className="flex items-center justify-center gap-3">
              <Mail size={18} className="text-[#e7c49a]" />
              <span>rebar.zebaree@gmail.com</span>
            </div>
            <div className="flex items-center justify-center gap-3">
              <Phone size={18} className="text-[#e7c49a]" />
              <span>+39 350 196 4857</span>
            </div>
          </div>
        </Reveal>

        <Reveal delay={0.16} className="mt-10 flex justify-center" y={22}>
          <a href="https://instagram.com/rebozee" target="_blank" rel="noreferrer">
            <Button className="rounded-full bg-[#f5efe6] px-8 text-black transition duration-500 hover:bg-[#e7c49a] hover:text-black">
              Follow on Instagram
            </Button>
          </a>
        </Reveal>
      </section>
    </div>
  );
}
