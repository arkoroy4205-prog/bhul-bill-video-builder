# Project inspection

Generated: Sat Nov 14 21:00:42 UTC 2026

## Files
bhul-bill-video-builder/SKILL.md
bhul-bill-video-builder/template/gen-sfx.mjs
bhul-bill-video-builder/template/package.json
bhul-bill-video-builder/template/src/Root.tsx
bhul-bill-video-builder/template/src/Video.tsx
bhul-bill-video-builder/template/src/brand.ts
bhul-bill-video-builder/template/src/chrome.tsx
bhul-bill-video-builder/template/src/data.ts
bhul-bill-video-builder/template/src/figures.tsx
bhul-bill-video-builder/template/src/index.ts
bhul-bill-video-builder/template/src/kit.tsx
bhul-bill-video-builder/template/src/rms.json
bhul-bill-video-builder/template/src/scenes.tsx
bhul-bill-video-builder/template/src/theme.ts
bhul-bill-video-builder/template/tools/make_rms.py
bhul-bill-video-builder/template/tsconfig.json

## package.json
{
  "name": "story-video",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@remotion/cli": "^4.0.539",
    "@types/react": "^19.3.0",
    "react": "^19.3.1",
    "react-dom": "^19.3.1",
    "remotion": "^4.0.539",
    "typescript": "^7.0.2"
  }
}

## SOURCE: src/Root.tsx
~~~tsx
import React from 'react';
import { Composition } from 'remotion';
import { Video } from './Video';
import { FPS, T } from './theme';
import { TOTAL } from './data';
export const Root: React.FC = () => (
  <Composition id="StoryVideo" component={Video} durationInFrames={T(TOTAL)} fps={FPS} width={1920} height={1080} />
);
~~~

## SOURCE: src/Video.tsx
~~~tsx
import React from 'react';
import { AbsoluteFill, Audio, Sequence, staticFile } from 'remotion';
import { T } from './theme';
import { TOTAL, SFX } from './data';
import { Overlay, useFonts } from './chrome';
import { SCENES } from './scenes';


export const Video: React.FC = () => {
  useFonts();
  return (
    <AbsoluteFill style={{ background: '#DDCFB7' }}>
      {SCENES.map(({ s, e, C: Comp }, i) => (
        <Sequence key={i} from={T(s)} durationInFrames={T(e) - T(s)}>
          <Comp t0={s} dur={T(e) - T(s)} />
        </Sequence>
      ))}
      <Overlay />
      <Audio src={staticFile('voice.mp3')} volume={1} />
      <Audio src={staticFile('sfx/pad.wav')} volume={0.1} />
      {SCENES.slice(1).map(({ s }, i) => (
        <Sequence key={`k${i}`} from={T(s)}>
          <Audio src={staticFile('sfx/tick.wav')} volume={0.16} />
        </Sequence>
      ))}
      {SFX.map(([n, s, v], i) => (
        <Sequence key={`x${i}`} from={T(s)}>
          <Audio src={staticFile(`sfx/${n}.wav`)} volume={v} />
        </Sequence>
      ))}
    </AbsoluteFill>
  );
};
~~~

## SOURCE: src/index.ts
~~~tsx
import { registerRoot } from 'remotion';
import { Root } from './Root';
registerRoot(Root);
~~~

## SOURCE: src/data.ts
~~~tsx
import rms from './rms.json';
export const RMS: number[] = rms as number[];
export const TOTAL = 59.0;   // voice duration + ~1.5 s

// captions: s/e in seconds (voice-aligned), text uses {red} for key words, \n for line break
export type Cap = { s: number; e: number; t: string; who?: string };
export const CAPS: Cap[] = [
  { s: 0.10, e: 1.77, t: 'তানভীর সারা {রাত} পড়ত।' },
  { s: 2.38, e: 3.96, t: 'তার স্বপ্ন ছিল {বিসিএস}।' },
  { s: 4.64, e: 7.13, t: 'বন্ধু শুভকে একদিন\n{চায়ের দোকানে} বলল,' },
  { s: 7.63, e: 9.88, t: '“{কাউকে} বলিস না,\nআমি বিসিএস দিচ্ছি।”', who: 'তানভীর' },
  { s: 10.48, e: 11.73, t: 'শুভ হেসে বলল,' },
  { s: 12.00, e: 13.81, t: '“আরে, আমি {কাউকে}\nবলব না।”', who: 'শুভ' },
  { s: 14.50, e: 16.86, t: 'তিন দিন পরে পাড়ার {চাচা}\nজিজ্ঞেস করলেন,' },
  { s: 17.36, e: 19.50, t: '“শুনলাম তুমি নাকি\n{বিসিএস} দিচ্ছ?”', who: 'চাচা' },
  { s: 20.20, e: 22.88, t: 'আত্মীয়রা বলল,\n“ভালো, {দেখি} কী হয়।”' },
  { s: 23.42, e: 25.81, t: 'বন্ধুরা {ঠাট্টা} করল,\n“তুই পারবি?”' },
  { s: 26.42, e: 29.56, t: 'তানভীর পড়তে বসলেই মনে হতো\n{সবাই} তাকিয়ে আছে।' },
  { s: 30.24, e: 30.92, t: '{চাপ} বাড়ল,' },
  { s: 31.58, e: 32.33, t: '{মনোযোগ} কমল।' },
  { s: 33.00, e: 34.32, t: 'পরীক্ষা শেষে {ফল} এল।' },
  { s: 35.02, e: 35.59, t: 'সে {পারেনি}।' },
  { s: 36.30, e: 36.98, t: 'সবাই বলল,' },
  { s: 37.58, e: 38.53, t: '“আমরা তো {জানতামই}।”' },
  { s: 39.20, e: 41.20, t: 'পরের বার তানভীর\n{কাউকে} কিছু বলেনি।' },
  { s: 41.88, e: 43.63, t: 'এক বছর {চুপচাপ} পড়ল।' },
  { s: 44.34, e: 47.53, t: 'ফল যেদিন এল, সেদিনই সে\nবাবার হাতে {রেজাল্ট} দিল।' },
  { s: 48.23, e: 49.25, t: '{শিক্ষাটা} সহজ।' },
  { s: 49.72, e: 52.61, t: 'স্বপ্ন নিয়ে তাড়াহুড়ো করে\nসবাইকে বলার {দরকার নেই}।' },
  { s: 53.28, e: 55.39, t: '{কাজ} আগে, {ঘোষণা} পরে।' },
];

export const SPEAK: Record<string, [number, number][]> = {
  tanvir: [[7.63, 9.88]],
  shuvo: [[12.0, 13.81]],
  chacha: [[17.36, 19.5]],
};

export const SCENE_TAGS: { s: number; e: number; tag: string }[] = [
  { s: 0, e: 4.3, tag: 'রাতের পড়া' },
  { s: 4.3, e: 14.1, tag: 'চায়ের দোকান' },
  { s: 14.1, e: 20.0, tag: 'তিন দিন পরে' },
  { s: 20.0, e: 23.2, tag: 'আত্মীয়রা' },
  { s: 23.2, e: 26.1, tag: 'বন্ধুরা' },
  { s: 26.1, e: 31.3, tag: 'পড়ার ঘর' },
  { s: 38.9, e: 44.1, tag: 'এক বছর পরে' },
  { s: 44.1, e: 48.0, tag: 'রেজাল্টের দিন' },
];

/** sound effects: [file in public/sfx without .wav, start second, volume] */
export const SFX: [string, number, number][] = [
  ['stamp', 35.0, 0.55],
  ['pop', 33.05, 0.35], ['pop', 2.45, 0.3], ['pop', 53.2, 0.3], ['pop', 54.55, 0.3],
];
~~~

## SOURCE: src/brand.ts
~~~tsx
// Page/channel name shown as a small watermark pill at the top of every frame.
// Leave empty ('') for NO watermark. Example: export const PAGE_NAME = 'আমার পেজ';
export const PAGE_NAME = '';
~~~

## SOURCE: src/theme.ts
~~~tsx
export const FPS = 30;
export const C = {
  cream: '#E6DDCB', beige: '#DDCFB7', ink: '#2B1D14', red: '#C8451E', card: '#F6EFE8',
  night: '#1C1917', navy: '#22284A', maroon: '#3B1B22', teal: '#2C483B', outline: '#2A1A12',
  paper: '#F6EBD9', tan: '#B9A07C',
};
export const T = (s: number) => Math.round(s * FPS);
export const bn = (n: number | string) => String(n).replace(/\d/g, (d) => '০১২৩৪৫৬৭৮৯'[Number(d)]);
export const clamp = { extrapolateLeft: 'clamp', extrapolateRight: 'clamp' } as const;
export const SPRING = { damping: 14, stiffness: 150, mass: 0.8 };
~~~

## SOURCE: src/chrome.tsx
~~~tsx
import React, { useEffect, useState } from 'react';
import { AbsoluteFill, Sequence, delayRender, continueRender, interpolate, spring, staticFile, useCurrentFrame } from 'remotion';
import { C, FPS, SPRING, T, clamp } from './theme';
import { CAPS, SCENE_TAGS, Cap } from './data';
import { PAGE_NAME } from './brand';

const FONTS: [string, string, string][] = [
  ['Hind', '600', 'hind-siliguri-bengali-600-normal.woff2'],
  ['Hind', '700', 'hind-siliguri-bengali-700-normal.woff2'],
  ['NSB', '800', 'noto-serif-bengali-bengali-800-normal.woff2'],
];
export const useFonts = () => {
  const [h] = useState(() => delayRender('fonts'));
  useEffect(() => {
    Promise.all(FONTS.map(([f, w, u]) => new FontFace(f, `url(${staticFile('fonts/' + u)})`, { weight: w }).load().then((ff) => document.fonts.add(ff))))
      .then(() => continueRender(h))
      .catch(() => continueRender(h));
  }, [h]);
};

// 540x960 portrait design space fitted and centered inside a 1920x1080 landscape composition
export const Stage: React.FC<{ dur: number; push?: number; children: React.ReactNode; bg?: string }> = ({ dur, push = 0.05, children, bg }) => {
  const f = useCurrentFrame();
  const sc = 1 + push * (f / dur);
  const op = interpolate(f, [0, 4], [0.55, 1], clamp);
  return (
    <AbsoluteFill style={{ background: bg ?? C.beige, opacity: op }}>
      <div style={{ position: 'absolute', inset: 0, transform: `scale(${sc})`, transformOrigin: '50% 58%' }}>
        <div style={{ width: 540, height: 960, position: 'absolute', left: '50%', top: '50%', transform: 'translate(-50%, -50%) scale(1.125)', transformOrigin: 'center', overflow: 'hidden' }}>
          <svg width={540} height={960} viewBox="0 0 540 960" style={{ position: 'absolute', left: 0, top: 0 }}>{children}</svg>
        </div>
      </div>
    </AbsoluteFill>
  );
};

const parse = (t: string) =>
  t.split('\n').map((line, i) => (
    <div key={i}>
      {line.split(/(\{[^}]+\})/).map((p, j) =>
        p.startsWith('{') ? <b key={j} style={{ color: C.red }}>{p.slice(1, -1)}</b> : <span key={j}>{p}</span>
      )}
    </div>
  ));

const Caption: React.FC<{ cap: Cap; len: number }> = ({ cap, len }) => {
  const f = useCurrentFrame();
  const sp = spring({ frame: f, fps: FPS, config: SPRING });
  const out = interpolate(f, [len - 5, len], [1, 0], clamp);
  return (
    <div style={{ position: 'absolute', left: 0, right: 0, bottom: 168, display: 'flex', justifyContent: 'center', opacity: Math.min(1, sp) * out, transform: `translateY(${(1 - sp) * 14}px) scale(${0.92 + 0.08 * sp})` }}>
      <div style={{ padding: '12px 24px', borderRadius: 20, background: C.card, boxShadow: '0 4px 14px #0003', fontFamily: 'Hind', fontWeight: 700, fontSize: 28, lineHeight: 1.38, color: C.ink, textAlign: 'center', whiteSpace: 'nowrap' }}>
        {cap.who && <div style={{ fontSize: 14, fontWeight: 600, color: '#8a7a68', marginBottom: 2 }}>● {cap.who}</div>}
        {parse(cap.t)}
      </div>
    </div>
  );
};

export const Overlay: React.FC = () => {
  const f = useCurrentFrame();
  const t = f / FPS;
  const tag = SCENE_TAGS.find((x) => t >= x.s + 0.15 && t < x.e);
  const ts = tag ? tag.s : 0;
  const tsp = spring({ frame: f - T(ts + 0.2), fps: FPS, config: SPRING });
  return (
    <AbsoluteFill>
      <div style={{ position: 'absolute', inset: 0 }}>
        <div style={{ width: 540, height: 960, position: 'absolute', left: '50%', top: '50%', transform: 'translate(-50%, -50%) scale(1.125)', transformOrigin: 'center' }}>
          {PAGE_NAME !== '' && (
            <div style={{ position: 'absolute', left: '50%', top: 26, transform: 'translateX(-50%)', padding: '6px 26px', borderRadius: 24, background: '#f1e8d8d9', fontFamily: 'NSB', fontWeight: 800, fontSize: 17, color: C.ink, whiteSpace: 'nowrap' }}>— {PAGE_NAME} —</div>
          )}
          {tag && (
            <div style={{ position: 'absolute', left: '50%', top: 68, transform: `translateX(-50%) scale(${0.85 + 0.15 * Math.min(1, tsp)})`, opacity: Math.min(1, tsp), padding: '7px 24px 8px', borderRadius: 30, background: '#F6EFE8', border: `3px solid ${C.red}`, boxShadow: '0 4px 12px #0003', fontFamily: 'Hind', fontWeight: 700, fontSize: 27, lineHeight: 1.25, color: C.ink, whiteSpace: 'nowrap' }}>
              <i style={{ display: 'inline-block', width: 13, height: 13, borderRadius: '50%', background: C.red, marginRight: 11, verticalAlign: 'middle' }} />{tag.tag}
            </div>
          )}
          {CAPS.map((c, i) => {
            const len = T(c.e + 0.18) - T(c.s);
            return (
              <Sequence key={i} from={T(c.s)} durationInFrames={len}>
                <Caption cap={c} len={len} />
              </Sequence>
            );
          })}
        </div>
      </div>
      <AbsoluteFill style={{ background: 'radial-gradient(ellipse at center, transparent 55%, rgba(30,15,5,0.22) 100%)', pointerEvents: 'none' }} />
      <AbsoluteFill style={{ opacity: 0.1, mixBlendMode: 'multiply', pointerEvents: 'none', backgroundImage: `url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='300' height='300'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' seed='${(f % 3) + 1}'/><feColorMatrix values='0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 0.9 0'/></filter><rect width='300' height='300' filter='url(%23n)'/></svg>")` }} />
    </AbsoluteFill>
  );
};
~~~

## SOURCE: src/scenes.tsx
~~~tsx
import React from 'react';
import { interpolate, spring, useCurrentFrame } from 'remotion';
import { C, FPS, SPRING, T, bn, clamp } from './theme';
import { Stage } from './chrome';
import { Person, Mini, Bubble, PersonP } from './figures';
import * as K from './kit';
const { mouthOpen, TANVIR, SHUVO, CHACHA, FATHER, useT, sp, breathe, blinkAt, RoomNight, DeskNight, DeskTop, RoomDay, RoomLiving, Relatives, Eye, Doc, Megaphone, StepCard } = K;
type SP = K.SP;
const O = K.O;

export const S0: React.FC<SP> = ({ t0, dur }) => {
  const { f, lf } = useT(t0);
  const dream = sp(lf(2.45));
  return (
    <Stage dur={dur}>
      <RoomNight f={f} />
      <DeskNight />
      <Person x={150} y={560 + breathe(f)} s={0.8} {...TANVIR} blink={blinkAt(f)} hand={[150, -62]} elbow={[70, -90]} />
      <DeskTop />
      <g transform={`translate(292,218) scale(${dream})`} opacity={Math.min(1, dream)}>
        <circle cx={0} cy={0} r={10} fill="#F6EFE8" transform="translate(-70,92)" />
        <circle cx={0} cy={0} r={16} fill="#F6EFE8" transform="translate(-48,64)" />
        <rect x={-62} y={-44} width={170} height={74} rx={36} fill="#F6EFE8" stroke={O} strokeWidth={3} />
        <rect x={-34} y={-28} width={114} height={42} rx={14} fill={C.red} />
        <text x={23} y={2} textAnchor="middle" fontFamily="NSB" fontWeight={800} fontSize={26} fill="#fff">বিসিএস</text>
      </g>
    </Stage>
  );
};

export const S1: React.FC<SP> = ({ t0, dur }) => {
  const { f, t } = useT(t0);
  const steam = (x: number, k: number) => (
    <path d={`M${x} 438 q-6 -12 0 -22 q6 -10 0 -22`} stroke="#ffffff" strokeWidth={3} fill="none" opacity={0.35 + 0.3 * Math.sin(f / 9 + k)} transform={`translate(0,${-((f + k * 9) % 26) * 0.5})`} />
  );
  return (
    <Stage dur={dur}>
      <rect width={540} height={960} fill="#DDCFB7" />
      <rect x={0} y={0} width={150} height={110} fill="#aebfc4" stroke="#8d7a5c" strokeWidth={6} />
      <rect x={62} y={0} width={6} height={110} fill="#8d7a5c" />
      <rect y={610} width={540} height={350} fill="#D3C7B1" />
      <rect y={606} width={540} height={6} fill="#c2b499" />
      <path d="M0 120 Q135 150 270 120 T540 120" stroke="#6b4a32" strokeWidth={3} fill="none" />
      {[40, 130, 220, 310, 400, 490].map((x, i) => (
        <circle key={i} cx={x} cy={128 + (i % 2 ? 6 : 14)} r={8} fill="#ffd98a" opacity={0.7 + 0.3 * Math.sin(f / 10 + i)} />
      ))}
      <rect x={30} y={470} width={170} height={190} rx={34} fill="#96A5AB" />
      <rect x={340} y={470} width={170} height={190} rx={34} fill="#a9a08a" />
      <Person x={112} y={520 + breathe(f)} s={0.82} {...TANVIR} blink={blinkAt(f)} mouth={mouthOpen('tanvir', t)} />
      <Person x={428} y={520 + breathe(f + 20)} s={0.82} flip {...SHUVO} blink={blinkAt(f + 40)} mouth={mouthOpen('shuvo', t)} smile={t > 10.48 && t < 12.0} />
      <rect x={196} y={500} width={150} height={14} rx={5} fill="#6B4A32" stroke={O} strokeWidth={3} />
      <rect x={256} y={514} width={30} height={190} fill="#5a3d28" />
      {[222, 298].map((x, i) => (
        <g key={i} transform={`translate(${x},446)`}>
          <rect width={34} height={50} rx={5} fill="#fbf6ee" stroke={O} strokeWidth={3} />
          <rect y={12} width={34} height={9} fill="#D4601F" />
          {i === 0 && <path d="M34 12 q18 4 0 24" fill="none" stroke="#fbf6ee" strokeWidth={7} />}
        </g>
      ))}
      {steam(239, 0)}{steam(315, 3)}
    </Stage>
  );
};

export const S2: React.FC<SP> = ({ t0, dur }) => {
  const { f, t, lf } = useT(t0);
  return (
    <Stage dur={dur}>
      <rect width={540} height={960} fill="#cfd8dc" />
      <rect x={-20} y={300} width={170} height={330} fill="#bfa98a" />
      <rect x={400} y={260} width={180} height={370} fill="#a89478" />
      {[0, 1, 2].map((r) => [0, 1].map((c) => (
        <rect key={`${r}${c}`} x={10 + c * 60} y={330 + r * 90} width={36} height={50} fill="#e9dcb8" opacity={0.85} />
      )))}
      {[0, 1, 2].map((r) => [0, 1].map((c) => (
        <rect key={`b${r}${c}`} x={430 + c * 56} y={290 + r * 95} width={36} height={52} fill="#d7c9a6" opacity={0.85} />
      )))}
      <rect x={0} y={610} width={540} height={350} fill="#b8a88a" />
      <rect x={0} y={606} width={540} height={8} fill="#a39475" />
      <rect x={262} y={250} width={10} height={360} fill="#4a3a2e" />
      <rect x={236} y={250} width={62} height={8} fill="#4a3a2e" />
      <Person x={120} y={520 + breathe(f)} s={0.8} pose="stand" {...(CHACHA as PersonP)} blink={blinkAt(f)} mouth={mouthOpen('chacha', t)} />
      <Person x={415} y={520 + breathe(f + 15)} s={0.8} pose="stand" flip {...TANVIR} blink={blinkAt(f + 30)} sad={t > 17.8} />
      {t > 17.3 && <Bubble x={200} y={238} text="?" w={54} delay={lf(17.36)} />}
    </Stage>
  );
};

export const S3: React.FC<SP> = (p) => <Relatives {...p} colors={['#B5543A', '#8f98a0', '#7a5c8f']} bubble="…" w={60} />;
export const S3b: React.FC<SP> = (p) => <Relatives {...p} colors={['#D9A93A', '#5f9d7a', '#c97f4a']} bubble="হা হা" w={92} laugh />;

export const S4: React.FC<SP> = ({ t0, dur }) => {
  const { f, t, lf } = useT(t0);
  const look: [number, number] = [155, 340];
  const eyes: [number, number][] = [[330, 190], [470, 270], [250, 160], [60, 300], [480, 380], [370, 330]];
  const fill = interpolate(t, [30.24, 31.2], [0, 0.92], clamp);
  return (
    <Stage dur={dur}>
      <RoomNight f={f} />
      <DeskNight />
      <Person x={150} y={560 + breathe(f)} s={0.8} {...TANVIR} blink={blinkAt(f)} sad hand={[150, -62]} elbow={[70, -90]} />
      <DeskTop />
      {eyes.map(([x, y], i) => <Eye key={i} x={x} y={y} d={T(26.7 - t0) + i * 14} look={look} f={f} />)}
      {t > 30.2 && (
        <g transform="translate(486,250)" opacity={Math.min(1, lf(30.24) / 6)}>
          <rect x={-18} y={0} width={36} height={220} rx={18} fill="#F6EFE8" stroke={O} strokeWidth={3} />
          <rect x={-12} y={214 - 208 * fill} width={24} height={208 * fill} rx={12} fill={C.red} />
          <text y={-10} textAnchor="middle" fontFamily="Hind" fontWeight={700} fontSize={20} fill="#F6EFE8">চাপ</text>
        </g>
      )}
    </Stage>
  );
};

export const S5: React.FC<SP> = ({ t0, dur }) => {
  const { f, t, lf } = useT(t0);
  const a = interpolate(t, [31.58, 32.6], [100, 25], clamp);
  const cardA = sp(lf(31.35));
  const outA = interpolate(t, [32.8, 33.1], [0, 1], clamp);
  const cardB = sp(lf(33.05), { damping: 11, stiffness: 120, mass: 0.9 });
  return (
    <Stage dur={dur} bg={C.cream}>
      <rect width={540} height={960} fill={C.cream} />
      <g transform={`translate(80,${250 - outA * 400}) scale(${Math.min(1, cardA)})`} opacity={1 - outA}>
        <rect width={380} height={170} rx={20} fill="#F6EFE8" stroke={O} strokeWidth={3} />
        <text x={30} y={50} fontFamily="Hind" fontWeight={700} fontSize={26} fill={C.ink}>মনোযোগ</text>
        <text x={350} y={50} textAnchor="end" fontFamily="NSB" fontWeight={800} fontSize={32} fill={C.red}>{bn(Math.round(a))}%</text>
        <rect x={30} y={90} width={320} height={26} rx={13} fill="#d9cbb2" />
        <rect x={30} y={90} width={(320 * a) / 100} height={26} rx={13} fill={C.teal} />
      </g>
      <g opacity={Math.min(1, cardB)} transform={`translate(0,${(1 - Math.min(1, cardB)) * -300})`}>
        <Doc x={130} y={180} />
        <text x={270} y={450} textAnchor="middle" fontFamily="NSB" fontWeight={800} fontSize={120} fill={C.red} opacity={0.5 + 0.5 * Math.sin(f / 6)}>?</text>
      </g>
    </Stage>
  );
};

export const S6: React.FC<SP> = ({ t0, dur }) => {
  const { f, t, lf } = useT(t0);
  const stampK = sp(lf(35.0), { damping: 9, stiffness: 220, mass: 0.7 });
  const docOut = interpolate(t, [36.1, 36.4], [1, 0], clamp);
  const showPeople = t > 36.2;
  return (
    <Stage dur={dur} bg={C.night}>
      <rect width={540} height={960} fill={C.night} />
      <g opacity={docOut}>
        <Doc x={130} y={190} />
        <g transform={`translate(270,400) rotate(-14) scale(${2.6 - 1.6 * Math.min(1, stampK)})`} opacity={Math.min(1, stampK)}>
          <circle r={86} fill="none" stroke={C.red} strokeWidth={14} />
          <path d="M-46 -46 L46 46 M46 -46 L-46 46" stroke={C.red} strokeWidth={22} strokeLinecap="round" />
        </g>
      </g>
      {showPeople && (
        <g>
          {['#B5543A', '#8f98a0', '#D9A93A', '#5f9d7a', '#7a5c8f'].map((c, i) => (
            <g key={i} opacity={sp(lf(36.25) - i * 3)} transform={`translate(0,${(1 - sp(lf(36.25) - i * 3)) * 30})`}>
              <Mini x={70 + i * 100} y={560} s={1.7} color={c} bob={0} />
              <Bubble x={75 + i * 100} y={300 + (i % 2) * 30} text={t > 37.5 ? '!' : '…'} w={56} delay={T(0.2) + i * 4 + T(36.25 - t0)} dark />
            </g>
          ))}
        </g>
      )}
    </Stage>
  );
};

export const S7: React.FC<SP> = ({ t0, dur }) => {
  const { f, t } = useT(t0);
  const filled = Math.max(0, Math.min(12, Math.floor((t - 39.3) / 0.34)));
  const done = sp(f - T(43.4 - t0));
  return (
    <Stage dur={dur}>
      <RoomDay />
      <rect x={215} y={500} width={330} height={14} rx={5} fill="#6B4A32" stroke={O} strokeWidth={3} />
      <rect x={222} y={514} width={14} height={140} fill="#5a3d28" />
      <rect x={510} y={514} width={14} height={140} fill="#5a3d28" />
      <Person x={150} y={560 + breathe(f)} s={0.8} {...TANVIR} blink={blinkAt(f)} smile={t > 41.0} hand={[150, -62]} elbow={[70, -90]} />
      <DeskTop lamp={false} />
      <g transform="translate(36,162)">
        {Array.from({ length: 12 }).map((_, i) => {
          const on = i < filled;
          const k = on ? sp(f - T(39.3 + i * 0.34 - t0), { damping: 10, stiffness: 200, mass: 0.6 }) : 0;
          return <rect key={i} x={(i % 6) * 78} y={Math.floor(i / 6) * 66} width={64} height={54} rx={12} fill={on ? C.teal : '#d3c7b1'} stroke={O} strokeWidth={2.5} transform={`translate(0,${(1 - Math.min(1, k)) * -8})`} />;
        })}
        {Array.from({ length: 12 }).map((_, i) => (
          <text key={i} x={(i % 6) * 78 + 32} y={Math.floor(i / 6) * 66 + 36} textAnchor="middle" fontFamily="Hind" fontWeight={700} fontSize={24} fill={i < filled ? '#fff' : '#9c8e76'}>{bn(i + 1)}</text>
        ))}
        <text x={234} y={-22} textAnchor="middle" fontFamily="NSB" fontWeight={800} fontSize={30} fill={C.red} opacity={Math.min(1, done)}>১ বছর</text>
      </g>
    </Stage>
  );
};

export const S8: React.FC<SP> = ({ t0, dur }) => {
  const { f, t, lf } = useT(t0);
  const badge = sp(lf(46.4), { damping: 10, stiffness: 160, mass: 0.8 });
  return (
    <Stage dur={dur}>
      <RoomLiving />
      <rect x={290} y={440} width={190} height={210} rx={34} fill="#8c6a4f" />
      <Person x={410} y={540 + breathe(f)} s={0.8} flip {...FATHER} blink={blinkAt(f)} smile={t > 46.9} />
      <Person x={100} y={520 + breathe(f + 12)} s={0.8} pose="stand" {...TANVIR} blink={blinkAt(f + 30)} smile={t > 46.9} arm="raise" hand={[130, -150]} elbow={[64, -112]} />
      <g transform="translate(214,386) rotate(-8)">
        <rect width={52} height={66} rx={5} fill="#F6EFE8" stroke={O} strokeWidth={3} />
        <rect x={8} y={12} width={36} height={6} rx={3} fill="#cdbfa8" /><rect x={8} y={28} width={26} height={6} rx={3} fill="#cdbfa8" />
        <path d="M14 48 l8 8 l16 -18" stroke={C.teal} strokeWidth={5} fill="none" strokeLinecap="round" strokeLinejoin="round" />
      </g>
      <g transform={`translate(270,190) scale(${Math.min(1, badge)})`} opacity={Math.min(1, badge)}>
        <rect x={-110} y={-30} width={220} height={60} rx={30} fill={C.teal} stroke={O} strokeWidth={3} />
        <text y={12} textAnchor="middle" fontFamily="NSB" fontWeight={800} fontSize={32} fill="#fff">উত্তীর্ণ ✔</text>
      </g>
    </Stage>
  );
};

export const S9: React.FC<SP> = ({ t0, dur }) => {
  const { f, lf } = useT(t0);
  const h = sp(lf(48.2));
  const m = sp(lf(49.8));
  const x = sp(lf(51.1), { damping: 9, stiffness: 220, mass: 0.7 });
  return (
    <Stage dur={dur} bg={C.cream}>
      <rect width={540} height={960} fill={C.cream} />
      <text x={270} y={190} textAnchor="middle" fontFamily="NSB" fontWeight={800} fontSize={54} fill={C.ink} opacity={Math.min(1, h)} transform={`translate(0,${(1 - Math.min(1, h)) * 20})`}>
        শিক্ষাটা <tspan fill={C.teal}>সহজ</tspan>
      </text>
      <Megaphone k={Math.min(1, m) * (1 + 0.02 * Math.sin(f / 8))} xk={x} />
    </Stage>
  );
};

export const S10: React.FC<SP> = ({ t0, dur }) => {
  const { lf } = useT(t0);
  const a = sp(lf(53.2)), ar = interpolate(lf(54.0), [0, 12], [0, 1], clamp), b = sp(lf(54.55));
  return (
    <Stage dur={dur} bg={C.cream}>
      <rect width={540} height={960} fill={C.cream} />
      <StepCard y={290} n="১" text="কাজ" k={a} />
      <g opacity={ar} transform={`translate(0,${(1 - ar) * -10})`}>
        <path d="M270 380 V470" stroke={C.ink} strokeWidth={8} strokeLinecap="round" />
        <path d="M244 450 L270 480 L296 450" stroke={C.ink} strokeWidth={8} fill="none" strokeLinecap="round" strokeLinejoin="round" />
      </g>
      <StepCard y={580} n="২" text="ঘোষণা" k={b} />
    </Stage>
  );
};

// List the scenes in time order. s/e are seconds on the final timeline; cut inside voice pauses.
// The last scene should end at TOTAL (see data.ts).
export const SCENES = [
  { s: 0.0, e: 4.3, C: S0 }, { s: 4.3, e: 14.1, C: S1 }, { s: 14.1, e: 20.0, C: S2 },
  { s: 20.0, e: 23.2, C: S3 }, { s: 23.2, e: 26.1, C: S3b }, { s: 26.1, e: 31.3, C: S4 },
  { s: 31.3, e: 34.7, C: S5 }, { s: 34.7, e: 38.9, C: S6 }, { s: 38.9, e: 44.1, C: S7 },
  { s: 44.1, e: 48.0, C: S8 }, { s: 48.0, e: 52.9, C: S9 }, { s: 52.9, e: 59.0, C: S10 },
];
~~~

## Archive summary
     7886  2026-11-01 17:35   bhul-bill-video-builder/template/src/kit.tsx
      207  2026-11-01 17:35   bhul-bill-video-builder/template/src/brand.ts
     2232  2026-11-01 09:20   bhul-bill-video-builder/template/gen-sfx.mjs
---------                     -------
    51578                     21 files
