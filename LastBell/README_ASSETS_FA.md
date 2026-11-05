# راهنمای پوشه‌های Assets زنگ آخر

این پوشه‌ها را کنار `index.html` و دقیقاً داخل `LastBell/assets/` نگه دار.

## کاراکترها — `assets/characters/`
`player`, `student`, `robot`, `ninja`, `astronaut`, `alien`, `sepehr`, `rashidi`

## اسلحه‌ها — `assets/weapons/`
`pistol`, `smg`, `shotgun`, `rifle`, `sniper`, `lmg`, `crossbow`, `launcher`, `flamethrower`, `ice`

## آیتم‌ها — `assets/items/`
`drink`

## نقشه‌ها — `assets/maps/`
`season1`, `season2`, `season3`, `season4`

## صداها — `assets/audio/`
- `sepehr.mp3`
- `terminator.mp4`

پسوندهای پشتیبانی‌شده برای مدل‌ها: `.glb`، `.gltf`، `.fbx`، `.obj`. برای دردسر کمتر `.glb` پیشنهاد می‌شود. نام فایل باید دقیقاً یکی از نام‌های بالا باشد؛ مثلاً `sepehr.glb`.

اگر فایل GLTF/FBX بافت یا فایل جانبی دارد، آن‌ها را هم با مسیر نسبی‌شان کنار مدل آپلود کن. در مدل نقشه، نام Meshهای برخورد را شامل `collider`، `wall`، `obstacle` یا `solid` بگذار.

بعد از افزودن فایل‌ها، پوشه `assets` را در همین مسیر `LastBell/assets/` در GitHub آپلود کن تا در نسخه HTTPS هم بارگذاری شوند.
