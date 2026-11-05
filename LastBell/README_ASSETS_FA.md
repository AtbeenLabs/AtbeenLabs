# راهنمای پوشه‌های Assets زنگ آخر

این پوشه‌ها را کنار `index.html` و دقیقاً داخل `LastBell/assets/` نگه دار.

## کاراکترها — `assets/characters/`
`player`, `student`, `robot`, `ninja`, `astronaut`, `alien`, `sepehr`, `rashidi`

## اسلحه‌ها — `assets/weapons/`
`pistol`, `smg`, `shotgun`, `rifle`, `sniper`, `lmg`, `crossbow`, `launcher`, `flamethrower`, `ice`

## آیتم‌ها — `assets/items/`
`drink`

## نقشه‌ها — `assets/maps/`
- `school.glb` = نقشه فصل اول
- `map.glb` = نقشه فصل دوم
- `season3.glb` و `season4.glb` = نقشه‌های فصل سوم و چهارم (وقتی آماده شدند)

## صداها — `assets/audio/`
- `sepehr.mp3` = صدای سپهر هنگام ضربه (در فهرست فعلی هنوز موجود نیست)
- `terminator.mp3` = موسیقی سپهر که با فاصله کم و زیاد می‌شود

پسوندهای پشتیبانی‌شده برای مدل‌ها: `.glb`، `.gltf`، `.fbx`، `.obj`. برای مدل‌های متحرک و نقشه، `.glb` پیشنهاد می‌شود.

اگر فایل GLTF/FBX بافت یا فایل جانبی دارد، آن‌ها را هم با مسیر نسبی‌شان کنار مدل آپلود کن. در مدل نقشه، نام Meshهای برخورد را شامل `collider`، `wall`، `obstacle` یا `solid` بگذار.

برای فایل‌های GLB فشرده‌شده با Meshopt یا Draco، نسخه فعلی بازی هر دو decoder را بارگذاری می‌کند.

فایل‌ها را داخل مسیر `LastBell/assets/` قرار بده تا GitHub Pages هم آن‌ها را با HTTPS ارائه کند.
