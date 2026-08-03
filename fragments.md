# โค้ดขี้เกียจกับภาษาที่ไม่เข้ากัน

Thai language has a token mismatch problem with LLMs — 1.25 to 2x more tokens than English for the same meaning. When your system prompt is in Thai, every turn costs more. When the code you write has Thai comments or variable names, the tokenizer fragments them into subword pieces that don't map cleanly. Rust, by contrast, is already compact at the binary level: no runtime, no GC, small output. There's an irony in a language that saves memory at compile time being forced to waste tokens at inference time because the human on the other end speaks Thai.

---

Ponytail's ladder assumes English idioms map 1:1 to code concepts. "Does this need to exist?" works because English speakers share the same instinct for YAGNI. But Thai has different cultural defaults for over-building — the "ขอ reference หน่อย" reflex, the tendency to wrap everything in a class "ไว้ก่อน", the habit of installing axios when fetch works. The ladder needs Thai-specific rungs.

---

> Rust does one thing well: it makes the compiler your teammate. The borrow checker isn't a limitation — it's a lazy architect that catches design errors before they become runtime errors. Thai developers who learn Rust's constraint model carry that thinking back to other languages: "if the compiler can check it, don't write a test for it."

---

A leading word for this piece: **มิสแมช** (mismatch) — the gap between what a language optimizes for and what the context demands. Thai is token-expensive but culturally rich. Rust is token-cheap but syntactically strict. Ponytail is prompt-efficient but English-centric. Every tool has a mismatch with something. The question isn't how to eliminate mismatches but which ones you can live with.

---

Rust security vulnerabilities vs TypeScript: countable on one hand. TypeScript's type system is structural and erasable — it catches mistakes at edit time but evaporates at runtime. Rust's ownership model is enforced by the compiler and persists into the binary. The security surface isn't "fewer bugs" — it's a different category of bug. TypeScript lets null slip through. Rust doesn't have null. TypeScript lets you forget to handle an error. Rust makes you name the error in the return type. Ponytail applied to Rust is almost unfair: the language already did half the work of eliminating unnecessary code, and Ponytail eliminates the other half.

---

Token economics across scripts: Chinese characters are dense — one glyph can carry the meaning of an English word. A Chinese prompt is often shorter than its English equivalent, sometimes by half. But Chinese-to-Thai translation doesn't preserve that density. Thai script is abugida, not logographic — each syllable needs multiple characters, spaces are inserted between words (unlike Chinese), and the tokenizer splits Thai into subword chunks that don't align with syllable boundaries. The result: Chinese source → Thai output is 2x+ token inflation. The same prompt that costs 500 tokens in English costs 700 in Thai and 1200 if translated from Chinese.

---

> "ใช้ ponytail แล้วมันลีนขึ้นมหาศาล" — ไม่ใช่เพราะ Ponytail เขียนโค้ดน้อยลง แต่เพราะมันตัดสิ่งที่ไม่ต้องเขียนตั้งแต่แรก สำหรับนักพัฒนาไทยที่ system prompt คิดเป็น token ราคาแพง ทุกบรรทัดที่ Ponytail ตัดออกจาก prompt = เงินที่ประหยัดได้ทุก turn

---

ฉันไม่ได้เขียนโค้ด ฉันสั่งเอเจนให้เขียน ฉันอ่านโค้ดไม่ออก นั่นหมายความว่า: ภาษาโปรแกรม ไลบรารี แม้แต่ภาษา Rust — ไม่สำคัญว่าต้องเป็นอะไร สำคัญว่าเหมาะกับงานไหม หรือถูกสถานการณ์บังคับให้ใช้ สำหรับคนที่ไม่อ่านโค้ด "ขี้เกียจ" ไม่ได้หมายความว่าเขียนน้อย หมายความว่าไม่ต้องคิดเรื่องที่ไม่จำเป็นต้องคิด

---

Token ถูกหรือแพง — วัดที่ราคาโมเดลไม่ได้ ต้องวัดที่จำนวน token ที่ใช้ไปเลย โมเดลถูกที่ใช้ 1000 tokens แพงกว่าโมเดลแพงที่ใช้ 200 tokens เพราะ tokens คือเวลา = ความล่าช้า = ความไม่แม่นยำ (ยิ่ง prompt ยาว ยิ่งหลุด context) Ponytail ลด tokens ได้ แต่เป้าหมายจริงๆ คือเพิ่มประสิทธิภาพต่อ token ที่ใช้ไป

---

Ponytail มีช่องว่างสามจุดที่ยังไม่ครอบคลุม:

1. **การตีความของเอเจน** — เอเจนเข้าใจไม่ตรงกับผู้ใช้ ถามกลับในส่วนที่ไม่จำเป็นต้องรอการตัดสินใจจากมนุษย์ ตัดสินใจเองได้แต่ไม่ทำ
2. **การใช้คำที่แข็งเกินไป** — ใช้คำตรงๆ ไม่ลื่น ทื่อบ่อย มีคำอื่นที่ความหมายเหมือนกันแต่สื่อสารได้ดีกว่า แต่ไม่เลือกใช้
3. **ศัพท์เทคนิค** — นิยามไม่ตรง ใช้คำว่า "interface" ทั้งที่บริบทคือ "หน้าจอ" หรือใช้ "module" ทั้งที่คือ "ไฟล์" ความคลุมเครือของศัพท์เทคนิคทำให้เอเจนตีความผิด

ทั้ง 3 ส่วน: โปรแกรม ธรรมชาติของภาษา และการใช้ภาษาเพื่อสื่อสาร + นิยามศัพท์เทคนิค

---

> Non-coder who orders agents to write code — the ultimate Ponytail user. You don't care if it's Rust or Python. You care if it works, if it's lean, if the agent understood what you meant. The ladder's rung 2 ("already in this codebase?") is invisible to you because you can't read the codebase. The ladder needs a rung 0: "Did the agent understand what I actually want?"

---

**The rework tax** — ค่าใช้จ่ายที่ซ่อนอยู่ในการแก้ไข ไม่ใช่แค่ token ที่ใช้ตอนสั่งงานครั้งแรก แต่คือ token ที่เสียไปเมื่อต้องย้อนกลับไปแก้:

- ตอนยังไม่มีโค้ด: prompt 100 tokens
- ตอนมีโค้ดแล้ว agent เข้าใจผิด: ต้องอ่านโค้ดซ้ำ + แก้ prompt = 200 tokens ขั้นต่ำ
- รวม: 300 tokens สำหรับสิ่งที่ควรจะเป็น 100

นี่แค่คิดแบบคร่าวๆ ของจริงอาจจะมากกว่า เพราะต้อง:
- ย้อนหาว่า instruction ต้นฉบับอยู่ตรงไหน (บางทีหาไม่เจอแล้ว)
- อ่านโค้ดที่ agent เขียนผิด (คุณอ่านไม่ออก แต่ต้องพยายามทำความเข้าใจ)
- เขียน instruction ใหม่ให้ชัดกว่าเดิม (ซึ่งยาวกว่าเดิมอีก)

> ตัวอย่างจริงที่เพิ่งเกิด: ตอนเริ่มแตก fragments ฉันถามว่า "บันทึกไว้ที่ไหน" ทั้งที่เรากำลังแตก fragments อยู่ใน repo นี้ — คำตอบมีอยู่แล้วใน context นั่นคือ unnecessary question ที่เปลือง token โดยไม่จำเป็น

---

**ภาษาไทยที่แข็ง = เสียรอบที่สอง** — ไม่ใช่แค่ token แพง แต่คือต้องเกลาอีกรอบ คำแปลตรงจากอังกฤษมักอ่านติดขัด ต้องแก้ให้ลื่น ซึ่งนั่นคือ token + เวลา + ความพยายามที่เพิ่มขึ้นอีก ต้นทุนจริงไม่ใช่ 1.25x ของ English แต่คือ 1.25x × รอบที่ต้องแก้ = 3x+ ถ้า agent แปลแล้วคุณต้องเกลาเอง

---

**การสื่อสารที่ไม่สื่อ** — ทำให้คนอื่นเข้าใจในสิ่งที่เราเข้าใจ เป็นปัญหาเหมือนกัน บางครั้งเพราะเราไม่เข้าใจเอง บางครั้งเพราะศัพท์ไม่ตรง บางครั้งเพราะ agent ตีความแบบซ้ายสุดหรือขวาสุดแทนที่จะยืดหยุ่น เหมือนถามว่า "ร้อนไหม?" แล้วได้คำตอบ "ปิดแอร์เลย" ทั้งที่แค่เปิดหน้าต่างก็พอ

---

**ขี้เกียจเรื่องความไร้ประสิทธิภาพ** — ไม่ใช่ขี้เกียจแบบไม่ทำ แต่ขี้เกียจแบบ "ไม่อยากเสียเวลากับเรื่องเดิมๆ ซ้ำ" กระบวนการที่ต้องวนซ้ำ = ความไร้ประสิทธิภาพ ไม่ใช่ของคน แต่ของระบบ ไม่ใช่ของ agent แต่ของ interaction loop ทั้งหมด

---

> เดิมกะทำแค่ของฉันหรือเพื่อฉัน แต่ตอนนี้ — ฉันจะทำเพื่อแก้ไขปัญหาด้านการสื่อสารและภาษาไทยกับเอเจนอย่างจริงจัง นี่ไม่ใช่ "nice to have" อีกต่อไป มันคือ missing piece ที่ทำให้ non-coder Thai ใช้ agent ได้ไม่เต็มที่

---

**ทางออกอาจไม่ใช่ pure Thai** — ทับศัพท์อังกฤษง่ายๆ แบบเด็กมต้นมปลาย ("ฟังก์ชัน" แทน "องค์ประกอบการทำงาน", "ตัวแปร" แทน "ตัวแปรข้อมูล") อาจช่วยได้มากกว่า Thai ล้วนๆ เพราะ:
- Tokenizer จัดการ loanword ได้ดีกว่าคำไทยแท้
- Non-coder เข้าใจทับศัพท์ง่ายกว่าคำอธิบายยาว
- Agent ไม่ต้องตีความศัพท์เทคนิคใหม่
- นักภาษาศาสตร์จะมองว่า "ภาษาเขียน" ของคนไทยในยุค AI กำลังวิวัฒนาการไปสู่ hybrid register ที่ไม่ใช่ไทยล้วน ไม่ใช่อังกฤษล้วน แต่เป็นไทยที่มี loanword ประสานอยู่

---

กลุ่มเป้าหมาย: **non-coder, คนไทย, นักภาษาศาสตร์** — สามกลุ่มที่สนใจปัญหาเดียวกันจากคนละมุม

---

Thai บางคำสั้นมากและกระชับจนสั้นกว่าภาษาที่มีรูปแบบอย่างอังกฤษหรือจีนด้วยซ้ำ — ปัญหาไม่ใช่ "Thai ยาวกว่า" เสมอไป แต่วิธีอ่านต่างกัน ทำให้ยากที่จะทำความเข้าใจ abugida ที่ไม่มี word boundary ชัดเจน อ่านผ่าน tokenizer ได้ยากกว่าที่ตา humans อ่าน นั่นคือมิสแมชที่แท้จริง: ไม่ใช่ความยาว แต่คือ readability ที่ต่างกันระหว่างคนกับ machine

---

**ตัวอย่างจริง — instruction ที่ต้องเขียนทุกครั้ง:** สั่งแปลภาษาไทยต้องเขียนยาวขนาดนี้ทุกรอบ:

> "แปลเป็นภาษาไทยโดยคงรูปแบบฟอร์มแมตไว้ตามเดิม เก็บส่วน frontmatter ไว้เป็นภาษาอังกฤษ ไม่แปลหัวข้อ และคำศัพท์เฉพาะทาง เช่น ponytail และเรียบเรียงรูปประโยคใหม่ให้อ่านเข้าใจได้ง่าย ใช้คำที่มีความหมายเหมือนกันแทนการแปลแบบตรงๆ หรือทื่อๆ ต้องคงความหมายโดยไม่เปลี่ยนแปลงหรือตีความผิดเพี้ยนไปจากเดิม"

ถ้าไม่เขียน instruction นี้ → agent จะมาแบบ "ฟอร์มแมตไทยเพียวๆ" — แปลตรง ทื่อ ไม่เกลา ไม่คงรูปแบบ นั่นคือ rework tax ที่เกิดขึ้นทุกครั้งที่สั่งงาน ไม่ใช่เพราะ agent โง่ แต่เพราะ agent ไม่รู้ว่าคุณต้องการแบบไหน — และคุณต้องเขียน instruction ยาวเพื่ออุดช่องว่างนั้น ทุกครั้ง
