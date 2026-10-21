
import React, { useEffect, useMemo, useState } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { CalendarDays, CloudSun, ExternalLink, Settings, Wifi, X, RefreshCw, CheckCircle2, AlertTriangle } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";

/*
  PURA VIDA PILAR TV DASHBOARD
  ------------------------------------------------------------------
  Productieopzet:
  TV-browser -> beveiligde Cloudflare Worker -> Lodgify API

  De Lodgify API-key hoort uitsluitend als Worker secret te bestaan.
  Plaats hem nooit in deze React-code, localStorage, URL of QR-code.
*/

const DEFAULTS = {
  propertyName: "Pura Vida Pilar",
  slogan: "Your Private Escape on the Costa Blanca",
  adapterUrl: "https://YOUR-WORKER.workers.dev/api/welcome",
  screenToken: "",
  guestPortalUrl: "https://guests.lodgify.com/",
  latitude: 37.8658,
  longitude: -0.7926,
  refreshMinutes: 15,
  fallbackGuest: "Welkom",
  wifiName: "Pura Vida Pilar Guest",
  primary: "#0D2038",
  surface: "#F5F0E7",
  accent: "#C8A96B",
  text: "#FFFDF8",
};

function formatDate(value) {
  if (!value) return "";
  const date = new Date(`${value}T12:00:00`);
  if (Number.isNaN(date.getTime())) return "";
  return new Intl.DateTimeFormat("nl-NL", {
    day: "numeric",
    month: "long",
    year: "numeric",
  }).format(date);
}

function formatTime(value) {
  if (!value) return "";
  const date = new Date(value);
  if (Number.isNaN(date.getTime())) return "";
  return new Intl.DateTimeFormat("nl-NL", { hour: "2-digit", minute: "2-digit" }).format(date);
}

function weatherLabel(code) {
  if (code === 0) return "Zonnig";
  if ([1, 2, 3].includes(code)) return "Licht bewolkt";
  if ([45, 48].includes(code)) return "Mistig";
  if ([51, 53, 55, 61, 63, 65, 80, 81, 82].includes(code)) return "Kans op regen";
  if ([95, 96, 99].includes(code)) return "Onweer";
  return "Mediterraan weer";
}

function localToday() {
  return new Intl.DateTimeFormat("en-CA", {
    timeZone: "Europe/Madrid",
    year: "numeric",
    month: "2-digit",
    day: "2-digit",
  }).format(new Date());
}

export default function PuraVidaWelcomeScreen() {
  const [config, setConfig] = useState(() => {
    try {
      return { ...DEFAULTS, ...JSON.parse(localStorage.getItem("pvp-tv-config") || "{}") };
    } catch {
      return DEFAULTS;
    }
  });
  const [settingsOpen, setSettingsOpen] = useState(false);
  const [reservation, setReservation] = useState(null);
  const [nextReservation, setNextReservation] = useState(null);
  const [weather, setWeather] = useState(null);
  const [status, setStatus] = useState("Dashboard wordt gestart");
  const [clock, setClock] = useState(new Date());
  const [connection, setConnection] = useState({ state: "idle", message: "" });
  const [loading, setLoading] = useState(false);
  const query = useMemo(() => new URLSearchParams(window.location.search), []);

  useEffect(() => {
    localStorage.setItem("pvp-tv-config", JSON.stringify(config));
  }, [config]);

  useEffect(() => {
    const timer = setInterval(() => setClock(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  useEffect(() => {
    let cancelled = false;
    async function loadWeather() {
      try {
        const url = `https://api.open-meteo.com/v1/forecast?latitude=${config.latitude}&longitude=${config.longitude}&current=temperature_2m,weather_code&daily=temperature_2m_max,temperature_2m_min&timezone=auto&forecast_days=1`;
        const response = await fetch(url, { cache: "no-store" });
        if (!response.ok) throw new Error("WEATHER_FAILED");
        const data = await response.json();
        if (!cancelled) {
          setWeather({
            temperature: Math.round(data.current.temperature_2m),
            code: data.current.weather_code,
            max: Math.round(data.daily.temperature_2m_max[0]),
            min: Math.round(data.daily.temperature_2m_min[0]),
          });
        }
      } catch {
        if (!cancelled) setWeather(null);
      }
    }
    loadWeather();
    const timer = setInterval(loadWeather, 30 * 60_000);
    return () => {
      cancelled = true;
      clearInterval(timer);
    };
  }, [config.latitude, config.longitude]);

  async function fetchWelcomeData({ test = false } = {}) {
    const manualGuest = query.get("guest");
    if (manualGuest && !test) {
      setReservation({
        guestName: manualGuest,
        checkIn: query.get("checkin") || "",
        checkOut: query.get("checkout") || "",
        language: query.get("language") || "nl",
      });
      setNextReservation(null);
      setStatus("Handmatige schermpreview");
      return;
    }

    if (!config.adapterUrl || !config.screenToken) {
      if (!test) {
        setReservation(null);
        setStatus("Open instellingen om de Lodgify-koppeling te configureren");
      }
      throw new Error("Adapter-URL of schermtoken ontbreekt");
    }

    setLoading(true);
    const controller = new AbortController();
    const timeout = setTimeout(() => controller.abort(), 10_000);

    try {
      const response = await fetch(config.adapterUrl, {
        method: "GET",
        cache: "no-store",
        credentials: "omit",
        headers: {
          Accept: "application/json",
          Authorization: `Bearer ${config.screenToken}`,
        },
        signal: controller.signal,
      });

      if (response.status === 401) throw new Error("Schermtoken is ongeldig");
      if (response.status === 403) throw new Error("Dashboarddomein is niet toegestaan");
      if (response.status === 429) throw new Error("Te veel verzoeken; probeer later opnieuw");
      if (!response.ok) throw new Error(`Adapterfout HTTP ${response.status}`);

      const payload = await response.json();
      if (!payload || !Array.isArray(payload.reservations)) throw new Error("Onverwacht antwoordformaat");

      const today = localToday();
      const active = payload.reservations.find((stay) => stay.checkIn <= today && today < stay.checkOut) || null;
      const next = payload.reservations.find((stay) => stay.checkIn > today) || null;

      if (!test) {
        setReservation(active);
        setNextReservation(next);
        setStatus(
          active
            ? `Automatisch bijgewerkt om ${formatTime(payload.generatedAt)}`
            : next
              ? `Geen actief verblijf · volgende check-in ${formatDate(next.checkIn)}`
              : "Geen actief verblijf gepland"
        );
      }
      return payload;
    } finally {
      clearTimeout(timeout);
      setLoading(false);
    }
  }

  useEffect(() => {
    let cancelled = false;
    async function refresh() {
      try {
        if (!cancelled) await fetchWelcomeData();
      } catch (error) {
        if (cancelled) return;
        setStatus(error?.name === "AbortError" ? "Lodgify-koppeling reageert niet op tijd" : "Lodgify-koppeling tijdelijk niet bereikbaar");
      }
    }
    refresh();
    const timer = setInterval(refresh, Math.max(5, Number(config.refreshMinutes) || 15) * 60_000);
    return () => {
      cancelled = true;
      clearInterval(timer);
    };
  }, [config.adapterUrl, config.screenToken, config.refreshMinutes, query]);

  async function testConnection() {
    setConnection({ state: "testing", message: "Verbinding testen…" });
    try {
      const payload = await fetchWelcomeData({ test: true });
      setConnection({
        state: "success",
        message: `Koppeling actief. ${payload.reservations.length} relevante reservering(en) ontvangen.`,
      });
    } catch (error) {
      setConnection({
        state: "error",
        message: error?.name === "AbortError" ? "De adapter reageerde niet binnen 10 seconden" : String(error?.message || "Verbinding mislukt"),
      });
    }
  }

  function update(key, value) {
    setConfig((current) => ({ ...current, [key]: value }));
  }

  const guestName = reservation?.guestName?.trim() || config.fallbackGuest;
  const greeting = reservation?.welcomeMessage || "Ontspan, vertraag en geniet van jullie verblijf aan de Costa Blanca.";
  const portalUrl = reservation?.guestPortalUrl || config.guestPortalUrl;
  const qrUrl = `https://api.qrserver.com/v1/create-qr-code/?size=320x320&margin=12&color=0D2038&bgcolor=FFFDF8&data=${encodeURIComponent(portalUrl)}`;

  return (
    <main
      className="relative min-h-screen overflow-hidden p-5 md:p-10"
      style={{
        color: config.text,
        background: `radial-gradient(circle at 85% 18%, ${config.accent}33, transparent 28%), linear-gradient(135deg, ${config.primary}, #07111f 78%)`,
      }}
    >
      <div className="absolute inset-0 opacity-[0.07]" style={{ backgroundImage: "linear-gradient(120deg, transparent 35%, #fff 35.15%, transparent 35.3%)" }} />

      <div className="relative z-10 mx-auto flex min-h-[calc(100vh-2.5rem)] max-w-[1600px] flex-col justify-between md:min-h-[calc(100vh-5rem)]">
        <header className="flex items-start justify-between gap-4">
          <div>
            <div className="text-xs font-semibold uppercase tracking-[0.42em]" style={{ color: config.accent }}>Private luxury residence</div>
            <h1 className="mt-3 text-3xl font-light tracking-wide md:text-5xl">{config.propertyName}</h1>
            <p className="mt-2 text-sm opacity-70 md:text-base">{config.slogan}</p>
          </div>
          <div className="flex gap-2">
            <Button aria-label="Vernieuwen" variant="outline" size="icon" onClick={() => fetchWelcomeData().catch(() => {})} className="border-white/20 bg-white/10 text-white hover:bg-white/20">
              <RefreshCw className={`h-5 w-5 ${loading ? "animate-spin" : ""}`} />
            </Button>
            <Button aria-label="Instellingen" variant="outline" size="icon" onClick={() => setSettingsOpen(true)} className="border-white/20 bg-white/10 text-white hover:bg-white/20">
              <Settings className="h-5 w-5" />
            </Button>
          </div>
        </header>

        <section className="grid items-center gap-8 py-8 md:grid-cols-[1.45fr_0.55fr]">
          <motion.div initial={{ opacity: 0, y: 22 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.8 }}>
            <p className="text-lg font-light uppercase tracking-[0.28em]" style={{ color: config.accent }}>Welkom bij Pura Vida Pilar</p>
            <h2 className="mt-4 max-w-4xl text-5xl font-light leading-[0.95] md:text-8xl lg:text-9xl">
              Fijn dat je er bent,<br />
              <span className="italic" style={{ color: config.accent }}>{guestName}</span>
            </h2>
            <p className="mt-7 max-w-2xl text-lg leading-relaxed text-white/72 md:text-2xl">{greeting}</p>

            {reservation?.checkIn && reservation?.checkOut && (
              <div className="mt-8 inline-flex items-center gap-4 rounded-2xl border border-white/15 bg-white/8 px-5 py-4 backdrop-blur-md">
                <CalendarDays className="h-6 w-6" style={{ color: config.accent }} />
                <div>
                  <div className="text-xs uppercase tracking-[0.24em] text-white/50">Verblijf</div>
                  <div className="mt-1 text-base md:text-lg">{formatDate(reservation.checkIn)} – {formatDate(reservation.checkOut)}</div>
                </div>
              </div>
            )}
          </motion.div>

          <Card className="border-white/15 bg-white/[0.08] text-white shadow-2xl backdrop-blur-xl">
            <CardContent className="p-6 md:p-8">
              <div className="flex items-center justify-between border-b border-white/10 pb-5">
                <div className="flex items-center gap-3">
                  <CloudSun className="h-7 w-7" style={{ color: config.accent }} />
                  <div>
                    <div className="text-xs uppercase tracking-[0.25em] text-white/45">Pilar de la Horadada</div>
                    <div className="mt-1 text-lg">{weather ? weatherLabel(weather.code) : "Weer wordt geladen"}</div>
                  </div>
                </div>
                <div className="text-right">
                  <div className="text-4xl font-light">{weather ? `${weather.temperature}°` : "--°"}</div>
                  {weather && <div className="text-xs text-white/50">{weather.min}° / {weather.max}°</div>}
                </div>
              </div>

              <div className="grid grid-cols-[1fr_auto] items-center gap-5 pt-6">
                <div>
                  <p className="text-xs uppercase tracking-[0.25em]" style={{ color: config.accent }}>Digitaal gastenportaal</p>
                  <p className="mt-3 text-sm leading-relaxed text-white/65">Scan voor verblijfdetails, praktische informatie en lokale tips.</p>
                  <a href={portalUrl} target="_blank" rel="noreferrer" className="mt-4 inline-flex items-center gap-2 text-sm hover:underline" style={{ color: config.accent }}>
                    Open portaal <ExternalLink className="h-4 w-4" />
                  </a>
                </div>
                <div className="rounded-2xl bg-[#FFFDF8] p-2 shadow-xl">
                  <img src={qrUrl} alt="QR-code naar het gastenportaal" className="h-28 w-28 md:h-40 md:w-40" />
                </div>
              </div>

              <div className="mt-6 flex items-center gap-3 rounded-xl bg-black/15 px-4 py-3 text-sm text-white/65">
                <Wifi className="h-4 w-4" />
                <span>Wifi: {config.wifiName}</span>
              </div>
            </CardContent>
          </Card>
        </section>

        <footer className="flex flex-wrap items-end justify-between gap-4 border-t border-white/10 pt-5 text-xs uppercase tracking-[0.2em] text-white/35">
          <span>{status}</span>
          <span>{new Intl.DateTimeFormat("nl-NL", { weekday: "long", day: "numeric", month: "long", hour: "2-digit", minute: "2-digit" }).format(clock)}</span>
        </footer>
      </div>

      <AnimatePresence>
        {settingsOpen && (
          <motion.aside initial={{ x: 460 }} animate={{ x: 0 }} exit={{ x: 460 }} transition={{ type: "spring", damping: 28, stiffness: 240 }} className="absolute right-0 top-0 z-30 h-full w-full max-w-md overflow-y-auto border-l border-white/10 bg-[#07111f]/95 p-6 shadow-2xl backdrop-blur-xl">
            <div className="flex items-center justify-between">
              <div>
                <div className="text-xs uppercase tracking-[0.25em]" style={{ color: config.accent }}>Beheer</div>
                <h3 className="mt-2 text-2xl font-light">Dashboardinstellingen</h3>
              </div>
              <Button variant="ghost" size="icon" onClick={() => setSettingsOpen(false)} className="text-white hover:bg-white/10"><X className="h-5 w-5" /></Button>
            </div>

            <div className="mt-7 space-y-5">
              {[
                ["propertyName", "Naam accommodatie", "text"],
                ["slogan", "Slogan", "text"],
                ["adapterUrl", "Lodgify-adapter URL", "url"],
                ["guestPortalUrl", "Standaard Guest Portal URL", "url"],
                ["fallbackGuest", "Standaard begroeting", "text"],
                ["wifiName", "Wifi-naam", "text"],
                ["latitude", "Breedtegraad", "number"],
                ["longitude", "Lengtegraad", "number"],
                ["refreshMinutes", "Verversing in minuten", "number"],
              ].map(([key, label, type]) => (
                <label key={key} className="block">
                  <span className="mb-2 block text-xs uppercase tracking-[0.18em] text-white/45">{label}</span>
                  <input type={type} value={config[key]} onChange={(e) => update(key, type === "number" ? Number(e.target.value) : e.target.value)} className="w-full rounded-xl border border-white/10 bg-white/10 px-4 py-3 text-white outline-none transition focus:border-[#C8A96B]" />
                </label>
              ))}

              <label className="block">
                <span className="mb-2 block text-xs uppercase tracking-[0.18em] text-white/45">Schermtoken</span>
                <input type="password" autoComplete="off" spellCheck="false" value={config.screenToken} onChange={(e) => update("screenToken", e.target.value.trim())} placeholder="Worker SCREEN_TOKEN" className="w-full rounded-xl border border-white/10 bg-white/10 px-4 py-3 text-white outline-none transition focus:border-[#C8A96B]" />
                <span className="mt-2 block text-xs leading-relaxed text-white/40">Lokaal opgeslagen op deze TV. De Lodgify API-key wordt hier nooit ingevoerd.</span>
              </label>

              <div className="grid grid-cols-3 gap-3">
                {[["primary", "Navy"], ["surface", "Ivory"], ["accent", "Gold"]].map(([key, label]) => (
                  <label key={key} className="block text-center text-xs text-white/45">
                    <input type="color" value={config[key]} onChange={(e) => update(key, e.target.value)} className="mb-2 h-12 w-full cursor-pointer rounded-lg border-0 bg-transparent" />
                    {label}
                  </label>
                ))}
              </div>

              <div className="rounded-2xl border border-white/10 bg-white/5 p-4">
                <Button type="button" onClick={testConnection} disabled={connection.state === "testing" || !config.adapterUrl || !config.screenToken} className="w-full bg-[#C8A96B] text-[#0D2038] hover:bg-[#D9BF87]">
                  {connection.state === "testing" ? "Verbinding testen…" : "Test Lodgify-koppeling"}
                </Button>
                {connection.message && (
                  <div className={`mt-3 flex items-start gap-2 text-sm ${connection.state === "success" ? "text-emerald-300" : connection.state === "error" ? "text-rose-300" : "text-white/55"}`}>
                    {connection.state === "success" ? <CheckCircle2 className="mt-0.5 h-4 w-4 shrink-0" /> : connection.state === "error" ? <AlertTriangle className="mt-0.5 h-4 w-4 shrink-0" /> : null}
                    <span>{connection.message}</span>
                  </div>
                )}
              </div>

              {nextReservation && (
                <div className="rounded-2xl border border-[#C8A96B]/25 bg-[#C8A96B]/10 p-4 text-sm text-white/65">
                  Volgende aankomst: {formatDate(nextReservation.checkIn)}. De naam wordt pas tijdens het actieve verblijf op het hoofdscherm getoond.
                </div>
              )}
            </div>
          </motion.aside>
        )}
      </AnimatePresence>
    </main>
  );
}

/*
======================================================================
CLOUDFLARE WORKER: src/index.js
======================================================================
Onderstaande backendcode hoort in een afzonderlijk Cloudflare Worker-project.
De veldnamen van Lodgify-responses kunnen per API-versie verschillen. Daarom
normaliseert de adapter meerdere bekende varianten en weigert hij responses
waarin geen herkenbare reserveringslijst aanwezig is.

export default {
  async fetch(request, env) {
    const url = new URL(request.url);

    if (request.method === "OPTIONS") return preflight(request, env);
    if (url.pathname === "/health" && request.method === "GET") {
      return respond({ ok: true }, 200, request, env);
    }
    if (url.pathname !== "/api/welcome" || request.method !== "GET") {
      return respond({ error: "Not found" }, 404, request, env);
    }
    if (!allowedOrigin(request, env)) return respond({ error: "Forbidden" }, 403, request, env);
    if (!safeEqual(bearer(request), env.SCREEN_TOKEN || "")) {
      return respond({ error: "Unauthorized" }, 401, request, env);
    }
    if (!env.LODGIFY_API_KEY || !env.LODGIFY_PROPERTY_ID) {
      return respond({ error: "Adapter is not configured" }, 503, request, env);
    }

    try {
      const today = madridDate(new Date());
      const from = shiftDate(today, -1);
      const to = shiftDate(today, 62);

      // Configure LODGIFY_BOOKINGS_PATH if the endpoint in your Lodgify account differs.
      const path = env.LODGIFY_BOOKINGS_PATH || "/v2/reservations/bookings";
      const apiUrl = new URL(path, "https://api.lodgify.com");
      apiUrl.searchParams.set("property_id", env.LODGIFY_PROPERTY_ID);
      apiUrl.searchParams.set("from", from);
      apiUrl.searchParams.set("to", to);

      const upstream = await fetch(apiUrl, {
        headers: {
          Accept: "application/json",
          "X-ApiKey": env.LODGIFY_API_KEY,
        },
        cf: { cacheTtl: 300, cacheEverything: true },
      });

      if (!upstream.ok) {
        console.error("Lodgify request failed", { status: upstream.status });
        return respond({ error: "Lodgify temporarily unavailable" }, 502, request, env);
      }

      const payload = await upstream.json();
      const bookings = getBookingArray(payload);
      if (!bookings) return respond({ error: "Unsupported Lodgify response" }, 502, request, env);

      const reservations = bookings
        .map(normalizeBooking)
        .filter(Boolean)
        .filter((b) => String(b.propertyId || "") === String(env.LODGIFY_PROPERTY_ID))
        .filter((b) => isDisplayableStatus(b.status))
        .filter((b) => b.checkOut > from && b.checkIn < to)
        .sort((a, b) => a.checkIn.localeCompare(b.checkIn))
        .slice(0, 2)
        .map((b) => ({
          guestName: cleanName(b.guestName),
          checkIn: b.checkIn,
          checkOut: b.checkOut,
          language: safeLanguage(b.language),
          welcomeMessage: welcomeText(safeLanguage(b.language)),
          guestPortalUrl: env.LODGIFY_GUEST_PORTAL_URL || "https://guests.lodgify.com/",
        }));

      return respond({
        property: env.PROPERTY_NAME || "Pura Vida Pilar",
        generatedAt: new Date().toISOString(),
        reservations,
      }, 200, request, env);
    } catch (error) {
      console.error("Adapter error", { message: String(error?.message || "error").slice(0, 160) });
      return respond({ error: "Adapter temporarily unavailable" }, 502, request, env);
    }
  },
};

function getBookingArray(payload) {
  if (Array.isArray(payload)) return payload;
  for (const key of ["items", "data", "bookings", "reservations", "results"]) {
    if (Array.isArray(payload?.[key])) return payload[key];
  }
  return null;
}

function normalizeBooking(raw) {
  const checkIn = dateOnly(raw.arrival || raw.check_in || raw.checkIn || raw.start_date || raw.date_from);
  const checkOut = dateOnly(raw.departure || raw.check_out || raw.checkOut || raw.end_date || raw.date_to);
  if (!checkIn || !checkOut || checkOut <= checkIn) return null;

  const guest = raw.guest || raw.customer || raw.contact || {};
  const first = guest.first_name || guest.firstName || raw.guest_first_name || "";
  const last = guest.last_name || guest.lastName || raw.guest_last_name || "";
  const display = guest.name || guest.full_name || raw.guest_name || `${first} ${last}`;

  return {
    propertyId: raw.property_id || raw.propertyId || raw.property?.id,
    guestName: display,
    checkIn,
    checkOut,
    status: raw.status || raw.booking_status || raw.state,
    language: guest.language || guest.locale || raw.language || raw.locale,
  };
}

function isDisplayableStatus(status) {
  const value = String(status || "").toLowerCase();
  if (!value) return true;
  return !["cancelled", "canceled", "declined", "deleted", "inquiry", "enquiry"].includes(value);
}

function cleanName(value) {
  return String(value || "Welkom")
    .replace(/[<>]/g, "")
    .replace(/[\\u0000-\\u001f\\u007f]/g, "")
    .replace(/\\s+/g, " ")
    .trim()
    .slice(0, 80) || "Welkom";
}

function safeLanguage(value) {
  const code = String(value || "en").toLowerCase().slice(0, 2);
  return ["nl", "en", "de", "fr", "es"].includes(code) ? code : "en";
}

function welcomeText(language) {
  return {
    nl: "Ontspan, vertraag en geniet van jullie verblijf aan de Costa Blanca.",
    en: "Relax, slow down and enjoy your stay on the Costa Blanca.",
    de: "Entspannen Sie sich und genießen Sie Ihren Aufenthalt an der Costa Blanca.",
    fr: "Détendez-vous et profitez de votre séjour sur la Costa Blanca.",
    es: "Relájense y disfruten de su estancia en la Costa Blanca.",
  }[language] || "Relax, slow down and enjoy your stay on the Costa Blanca.";
}

function dateOnly(value) {
  if (!value) return "";
  const text = String(value);
  const match = text.match(/^\\d{4}-\\d{2}-\\d{2}/);
  if (match) return match[0];
  const date = new Date(value);
  return Number.isNaN(date.getTime()) ? "" : date.toISOString().slice(0, 10);
}

function shiftDate(iso, days) {
  const date = new Date(`${iso}T12:00:00Z`);
  date.setUTCDate(date.getUTCDate() + days);
  return date.toISOString().slice(0, 10);
}

function madridDate(date) {
  return new Intl.DateTimeFormat("en-CA", {
    timeZone: "Europe/Madrid", year: "numeric", month: "2-digit", day: "2-digit",
  }).format(date);
}

function bearer(request) {
  const value = request.headers.get("authorization") || "";
  return value.startsWith("Bearer ") ? value.slice(7) : "";
}

function safeEqual(a, b) {
  const x = new TextEncoder().encode(String(a));
  const y = new TextEncoder().encode(String(b));
  const size = Math.max(x.length, y.length);
  let diff = x.length ^ y.length;
  for (let i = 0; i < size; i++) diff |= (x[i] || 0) ^ (y[i] || 0);
  return diff === 0;
}

function allowedOrigin(request, env) {
  const expected = String(env.ALLOWED_ORIGIN || "").replace(/\\/$/, "");
  const actual = String(request.headers.get("origin") || "").replace(/\\/$/, "");
  return Boolean(expected) && expected === actual;
}

function preflight(request, env) {
  if (!allowedOrigin(request, env)) return new Response(null, { status: 403 });
  return new Response(null, { status: 204, headers: cors(env) });
}

function cors(env) {
  return {
    "Access-Control-Allow-Origin": env.ALLOWED_ORIGIN,
    "Access-Control-Allow-Methods": "GET,OPTIONS",
    "Access-Control-Allow-Headers": "Authorization,Content-Type",
    "Access-Control-Max-Age": "600",
    Vary: "Origin",
  };
}

function respond(body, status, request, env) {
  const headers = {
    "Content-Type": "application/json; charset=utf-8",
    "Cache-Control": "no-store, max-age=0",
    "X-Content-Type-Options": "nosniff",
    "Referrer-Policy": "no-referrer",
    "Content-Security-Policy": "default-src 'none'; frame-ancestors 'none'",
  };
  if (allowedOrigin(request, env)) Object.assign(headers, cors(env));
  return new Response(JSON.stringify(body), { status, headers });
}

======================================================================
WRANGLER.CONFIG.JSONC
======================================================================
{
  "$schema": "node_modules/wrangler/config-schema.json",
  "name": "pura-vida-pilar-lodgify-adapter",
  "main": "src/index.js",
  "compatibility_date": "2026-10-21",
  "vars": {
    "ALLOWED_ORIGIN": "https://welcome.puravidapilar.com",
    "PROPERTY_NAME": "Pura Vida Pilar",
    "LODGIFY_PROPERTY_ID": "REPLACE_WITH_PROPERTY_ID",
    "LODGIFY_BOOKINGS_PATH": "/v2/reservations/bookings",
    "LODGIFY_GUEST_PORTAL_URL": "https://guests.lodgify.com/"
  }
}

Set secrets, never regular variables:
  npx wrangler secret put LODGIFY_API_KEY
  npx wrangler secret put SCREEN_TOKEN

Generate the screen token:
  openssl rand -hex 32

Deploy:
  npx wrangler deploy

Verify first with the exact booking endpoint and field names shown in your Lodgify API documentation/account.
*/
