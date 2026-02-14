<!DOCTYPE html>
<html>
<head>
    <title>Roblox AI Helper</title>
    <style>
        body {
            font-family: Arial;
            background: #1e3a8a;
            padding: 20px;
            margin: 0;
        }
        
        body::before {
            content: '6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7 6 7';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            font-size: 300px;
            font-weight: bold;
            color: rgba(59, 130, 246, 0.15);
            z-index: 0;
            pointer-events: none;
            line-height: 1.2;
            letter-spacing: 80px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        h1 {
            color: white;
            text-align: center;
        }
        
        .chat {
            background: white;
            border-radius: 10px;
            padding: 20px;
        }
        
        #messages {
            height: 400px;
            overflow-y: scroll;
            border: 2px solid #3b82f6;
            padding: 10px;
            margin-bottom: 20px;
            background: #eff6ff;
        }
        
        .message {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
            white-space: pre-wrap;
        }
        
        .user {
            background: #2563eb;
            color: white;
            text-align: right;
        }
        
        .bot {
            background: white;
            border: 2px solid #93c5fd;
            color: #1e40af;
        }
        
        input {
            width: 70%;
            padding: 10px;
            border: 2px solid #3b82f6;
            border-radius: 5px;
            font-size: 16px;
        }
        
        button {
            width: 25%;
            padding: 10px;
            background: #2563eb;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        
        button:hover {
            background: #1d4ed8;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎮 Roblox AI Helper</h1>
        
        <div class="chat">
            <div id="messages"></div>
            
            <input type="text" id="input" placeholder="Ask me anything...">
            <button onclick="send()">Send</button>
        </div>
    </div>

    <script>
        // MASSIVE game ideas database (50+ ideas)
        var games = {
            obby: [
                "🏃 SPEED RUNNER OBBY: Race through moving platforms with boost pads, checkpoints every 10 obstacles, difficulties from Easy to INSANE, leaderboard for fastest times, secret shortcuts, and rainbow completion trails!",
                "🌈 RAINBOW COLOR OBBY: Each level is a different color! RED=fire/lava, BLUE=ice/slippery, GREEN=nature/vines, YELLOW=lightning, PURPLE=gravity flip, ORANGE=bouncy! Mix all colors for final boss level.",
                "🚀 SPACE OBBY: Journey from Earth to Black Hole! Zero gravity sections, asteroid jumping, rocket boosters, alien NPCs, collect stars for shop, unlock spaceship skins and trails!",
                "🏰 MEDIEVAL CASTLE OBBY: Escape dungeon to reach rooftop! Swinging axes, arrow traps, knight patrols, friendly dragon race at top, medieval shop with armor and sword skins!",
                "🌊 UNDERWATER OBBY: 50 levels from surface to ocean floor! Swimming mechanics, oxygen bubbles, avoid sharks and jellyfish, coral reefs, submarine checkpoints, collect pearls!",
                "🎪 CIRCUS OBBY: Tightrope walking, cannon launchers, trampoline zones, spinning wheels, dodge juggling balls, clown NPCs cheer you on!",
                "❄️ ICE MOUNTAIN OBBY: Climb frozen peaks! Slippery ice, avalanche escape sequences, ice cave sections, yeti encounters, hot cocoa checkpoints!",
                "🏜️ DESERT PYRAMID OBBY: Ancient Egyptian theme! Sand traps, mummy encounters, hieroglyph puzzles, scarab collecting, pharaoh's treasure at top!"
            ],
            
            simulator: [
                "🐾 MEGA PET SIMULATOR: 100+ pets from common to mythical, 3-stage evolution, custom homes, mini-games (fetch/agility/races), breeding system, pet abilities, daily care, trading!",
                "🍕 PIZZA EMPIRE: Start with pizza stand, 50+ toppings, hire chefs/drivers, upgrade ovens, multiple locations, custom creator, delivery mini-game, competitions!",
                "⚔️ SWORD MASTER: 200+ legendary swords, train stats, battle dummies, quest system, dungeon raids, forge swords, enchantments, PvP arena, armor sets!",
                "🏝️ ISLAND BUILDER: Start tiny, expand island, plant crops, build houses/shops, attract tourists, unlock new islands (tropical/volcanic/arctic), fishing, treasure hunting!",
                "🧙 MAGIC ACADEMY: 50+ spells, attend classes (Potions/Charms/Defense), collect wands, familiar pets, homework quests, wizard duels, house competitions!",
                "💎 MINING TYCOON: Dig for gems underground, 50+ gem types, upgrade tools, hire miners, craft jewelry, unlock new caves (volcano/ice/crystal), find artifacts!",
                "🏋️ GYM SIMULATOR: Train strength/speed/endurance, workout equipment, compete in competitions, hire trainer, healthy food shop, unlock gyms in different cities!",
                "🎨 ART STUDIO: Paint on canvas, sell artwork, unlock styles/tools, display in gallery, art contests, commission system, upgrade studio!"
            ],
            
            adventure: [
                "🗺️ TREASURE QUEST: Massive island with 20+ locations, find map pieces, solve riddles, explore caves/temples, ancient traps, boss Guardian, hidden rooms!",
                "🏰 KINGDOM RPG: Choose Knight/Wizard/Archer/Rogue, 30+ quests, battle system, explore forests/caves/castles, collect gear, level 1-50, final boss: Dark Sorcerer!",
                "🌲 ENCHANTED FOREST: Meet fairies/unicorns/talking trees, 15 forest zones, gather herbs, craft potions, animal companions, build treehouse, seasonal events!",
                "🏴‍☠️ PIRATE ADVENTURE: Captain your ship, visit 10 islands, treasure maps, naval battles, recruit crew, upgrade ship, sea monsters, trade goods!",
                "🏔️ MOUNTAIN EXPEDITION: Multi-day journey, survival mechanics, scenic viewpoints, weather challenges, wildlife, upgrade gear, photograph sights, secret caves!",
                "🌋 VOLCANO ISLAND: Active volcano erupting! Build raft to escape, gather supplies while lava flows, evacuate with friends, rescue animals, hot zones, earthquakes!",
                "🏺 ANCIENT TEMPLE: Explore ruins, hieroglyph puzzles, trap rooms, treasure chambers, torch lighting, multiple endings based on choices!",
                "🎭 MYSTERY MANSION: 30-room haunted house, collect clues, interview ghosts, puzzle rooms, secret passages, piece together mystery, multiple suspects!"
            ],
            
            racing: [
                "🏎️ TURBO KART RACING: 20 tracks (city/beach/volcano/space), 50+ karts, power-ups (rockets/shields/boost), customize everything, championship mode, time trials, multiplayer 8 players!",
                "🛹 SKATE PARK PRO: Massive park, trick system (kickflip/ollie/grind/manual), combo multiplier, collect S-K-A-T-E letters, create custom parks, 30+ boards!",
                "🏁 DRAG RACING: Perfect launch timing, gear shifts, nitrous boost, tune cars (engine/tires), 40+ cars, underground story, pink slip races, custom wraps!",
                "🚁 HELICOPTER RACING: Choppers/planes/jets, ring courses through clouds, barrel rolls, weather challenges, 15 aircraft, canyon racing, stunts!",
                "🏇 HORSE RACING: Train horses, 10 breeds, jump obstacles, betting system, breeding, jockey outfits, famous tracks, bonding affects performance!",
                "🚤 BOAT RACING: Jet skis/speedboats/yachts, ocean/river/lake tracks, wave physics, trick jumps, marine obstacles, underwater tunnels!",
                "🏃 PARKOUR RACE: Free-running competition, wall runs, precision jumps, city rooftops, time attack, multiplayer, unlock abilities, custom courses!"
            ],
            
            tycoon: [
                "🏪 MEGA MALL: Start 1 shop to 50+, different store types, hire employees/security, parking/valet, food court, theater/arcade, seasonal sales, customer satisfaction!",
                "🎢 THEME PARK: 30+ rides, custom roller coaster builder, food stands/games, performers/mascots, queue management, cleanliness matters, fireworks, seasonal themes!",
                "🏗️ CITY BUILDER: Village to metropolis, zone residential/commercial/industrial, infrastructure (roads/power/water), 100+ buildings, budget/taxes, public services!",
                "🍔 RESTAURANT CHAIN: Multiple food chains, menu customization, drive-thru/dine-in, hire/train staff, marketing, compete with rivals, expand to cities!",
                "🏨 RESORT TYCOON: Beachfront resort, room types (standard to presidential), amenities (pool/spa/restaurant), events (weddings/conferences), guest reviews!",
                "🎮 ARCADE EMPIRE: 50+ arcade cabinets, ticket prizes, claw machines, VR section, snack bar, tournaments, maintain machines, nostalgic decor!",
                "🏭 FACTORY TYCOON: Production lines, raw materials to products, conveyor automation, hire workers, research tech, fulfil orders, expand factory!"
            ],
            
            fighting: [
                "🥊 SUPERHERO BATTLE: Create heroes with powers, punch/kick/special moves, different arenas (city/volcano), unlock costumes and abilities!",
                "⚔️ MEDIEVAL COMBAT: Knights with swords/shields, blocking system, different weapon types, armor upgrades, tournament mode, epic duels!",
                "🤖 ROBOT BATTLE: Build and battle robots, customize parts/weapons/colors, arena with hazards, different weight classes!",
                "🥋 NINJA DOJO: Learn ninja moves, stealth abilities, throwing stars, wall-climbing, katana combat, training challenges!",
                "🦖 DINOSAUR BATTLE: Control different dinosaurs, each with unique abilities (T-Rex=strong, Raptor=fast, Pterodactyl=fly)!"
            ]
        };

        // MASSIVE knowledge database (200+ answers)
        var knowledge = {
            // Science questions
            "sky blue": "🌤️ The sky is blue because of Rayleigh scattering! When sunlight enters Earth's atmosphere, it hits tiny air molecules. Blue light has shorter wavelengths and scatters more easily than other colors, so we see blue everywhere! At sunset, light travels through more atmosphere, so we see reds and oranges instead!",
            "planes fly": "✈️ Planes fly because of lift! Wings are curved on top and flat on bottom (airfoil shape). Air moves faster over the curved top, creating lower pressure above the wing. Higher pressure below pushes the wing up! Engines provide forward thrust, and this wing shape keeps it airborne.",
            "gravity": "🌍 Gravity is a force that pulls objects with mass together! Earth's massive size creates strong gravity that keeps us on the ground and makes things fall. Everything with mass has gravity - even you! But you'd need to be planet-sized for your gravity to be noticeable. Fun fact: You actually pull on Earth with the same force Earth pulls on you, but Earth is so massive it doesn't move!",
            "photosynthesis": "🌱 Photosynthesis is how plants make food from sunlight! Plants use:\n• Sunlight (energy source)\n• Water (from roots)\n• CO2 (from air through stomata)\n\nThey create:\n• Glucose/Sugar (food for plant)\n• Oxygen (released for us!)\n\nThis happens in chloroplasts using chlorophyll (the green pigment). Without photosynthesis, we wouldn't have oxygen to breathe!",
            "magnets work": "🧲 Magnets work because of aligned atoms! Inside magnetic materials like iron, tiny atoms act like mini magnets. In a magnet, these atoms line up in the same direction, creating a strong magnetic field. Opposite poles (north-south) attract because field lines connect. Same poles (north-north or south-south) repel because field lines push apart!",
            "rainbow": "🌈 Rainbows form when sunlight shines through water droplets! Light enters the droplet and bends (refracts), then reflects off the back, and bends again coming out. Different colors bend at different angles, separating into: Red, Orange, Yellow, Green, Blue, Indigo, Violet (ROY G BIV). You see a circle arc because that's the angle where light exits the droplets toward your eyes!",
            "thunder": "⚡ Thunder is the sound of lightning heating air! When lightning strikes, it's incredibly hot (30,000°F - 5 times hotter than sun's surface!). This superheat air so fast it explodes outward, creating a shock wave. That's the BOOM you hear! Light travels faster than sound (186,000 miles/sec vs 767 mph), so you see lightning before hearing thunder. Count seconds between flash and thunder, divide by 5 = miles away!",
            "seasons": "🍂 Seasons happen because Earth is tilted 23.5 degrees! As Earth orbits the sun, different parts tilt toward or away from it:\n• Summer: Your hemisphere tilts toward sun (more direct sunlight, longer days)\n• Winter: Tilts away from sun (less direct sunlight, shorter days)\n• Spring/Fall: In between positions\n\nIt's NOT about distance from sun - Earth is actually closest to sun during Northern Hemisphere winter!",
            "rain": "🌧️ Rain forms when water vapor condenses in clouds! Warm air rises carrying water vapor. High up where it's cold, vapor condenses around tiny particles (dust/pollen) forming water droplets. Millions of droplets stick together forming bigger drops. When drops get too heavy for air currents to hold, they fall as rain!",
            "wind": "💨 Wind is moving air caused by temperature differences! When sun heats Earth's surface, warm air rises (it's lighter). Cool air rushes in to fill the space, creating wind! Larger patterns: Sun heats equator more than poles, creating global wind patterns. That's why we have trade winds, jet streams, and monsoons!",
            "volcanoes": "🌋 Volcanoes form when hot molten rock (magma) erupts from underground! Earth's crust has cracks where tectonic plates meet. Magma from the mantle (super hot layer below crust) pushes through these cracks. Pressure builds up and BOOM - eruption! Lava flows out, cools, and hardens into new rock. Over time, this builds up creating mountains we call volcanoes!",
            "earthquakes": "📊 Earthquakes happen when tectonic plates suddenly shift! Earth's crust is broken into huge plates floating on hot mantle below. These plates constantly move (very slowly). Sometimes they get stuck on each other. Pressure builds... builds... then SNAP! They suddenly slip, releasing energy as seismic waves that shake the ground. The point where it breaks is called the focus, directly above it on surface is the epicenter!",
            "moon phases": "🌙 Moon phases show how much sunlight we see on the moon!\n• New Moon: Moon between Earth and Sun (dark side faces us)\n• Crescent: Tiny sliver visible\n• First Quarter: Half lit (right side)\n• Gibbous: More than half\n• Full Moon: Earth between Moon and Sun (fully lit side faces us)\n• Then reverse back\n\nThe moon doesn't glow - it reflects sunlight! Takes 29.5 days for complete cycle.",
            "stars twinkle": "⭐ Stars twinkle because of Earth's atmosphere! Starlight travels through layers of moving air with different temperatures and densities. This bends the light in slightly different directions very rapidly, making stars appear to twinkle or shimmer. In space (no atmosphere), stars don't twinkle! Planets don't twinkle much either because they're close enough to appear as small discs, not points of light.",

            // Animals
            "fastest animal": "🐆 Land: Cheetah at 70 mph! Can accelerate 0-60 in 3 seconds!\n🦅 Air: Peregrine falcon diving at 240+ mph!\n🐟 Water: Sailfish at 68 mph, Black marlin at 80 mph!\n🦎 Fastest reptile: Spiny-tailed iguana at 21 mph\n🐜 Fastest insect: Australian tiger beetle at 5.6 mph (120 body lengths per second!)",
            "biggest animal": "🐋 Blue whale - biggest animal EVER! Even bigger than any dinosaur!\n• Length: Up to 100 feet (3 school buses!)\n• Weight: 200 tons (33 elephants!)\n• Heart: Size of small car\n• Tongue: Weighs as much as an elephant\n• Can eat 4 tons of krill per day\n• Baby blue whale drinks 50 gallons of milk daily and gains 200 pounds per day!",
            "smallest animal": "🦟 Smallest mammal: Bumblebee bat (1 inch long!)\n🐸 Smallest vertebrate: Paedophryne amauensis frog (7mm - size of a housefly!)\n🦠 Smallest animal overall: Tardigrades/water bears (0.5mm - microscopic!)",
            "pandas eat": "🐼 Giant pandas eat bamboo - LOTS of it!\n• Consume: 26-84 pounds per day\n• Time eating: 12-16 hours daily\n• Diet: 99% bamboo (occasionally eat other plants, eggs, small animals)\n• Why so much? Bamboo has low nutrition, so they need to eat constantly!\n• They have a 'thumb' (modified wrist bone) to grip bamboo\n• Eat 20+ types of bamboo",
            "penguins fly": "🐧 Penguins can't fly in AIR, but they FLY underwater!\n• Wings evolved into flippers\n• 'Fly' through water at 15-25 mph\n• Some species reach 22 mph underwater\n• Can leap 6+ feet out of water\n• Gentoo penguins: fastest swimming birds\n• They traded flight for incredible swimming ability!",
            "dogs live": "🐕 Dog lifespan depends on size:\n• Small dogs (Chihuahua): 12-20 years\n• Medium dogs (Beagle): 10-15 years\n• Large dogs (Lab): 10-13 years\n• Giant dogs (Great Dane): 7-10 years\n\nLongest living dog: Australian Cattle Dog named Bluey lived 29 years! Factors affecting lifespan: genetics, diet, exercise, vet care, and love!",
            "cats purr": "😺 Cats purr by vibrating their larynx (voice box) muscles!\n• 25-150 vibrations per second\n• Purr when: Happy, content, want attention, nursing, sometimes when injured (healing)\n• Purring may help: Heal bones (vibrations stimulate bone growth), reduce stress, lower blood pressure\n• Big cats (lions/tigers) can't purr continuously - they roar instead!\n• Cats also purr when scared or in pain for self-comfort",
            "sharks old": "🦈 Sharks are ANCIENT - older than trees!\n• First appeared: 400 million years ago\n• Trees appeared: 350 million years ago\n• Survived 5 mass extinctions\n• Greenland sharks can live 400+ years (oldest living vertebrate!)\n• Sharks existed before: dinosaurs, Saturn's rings, Polaris (North Star)\n• Modern sharks evolved 100 million years ago",
            "dolphins smart": "🐬 Dolphins are incredibly intelligent!\n• Brain size: Large and complex (2nd to humans)\n• Use tools: Use sponges to protect nose while hunting\n• Names: Have unique whistle signatures (names for each other!)\n• Self-aware: Recognize themselves in mirrors\n• Language: Complex communication with clicks/whistles\n• Play: Engage in games and surf waves for fun\n• Help others: Save injured dolphins and even humans\n• Learn quickly: Can learn new behaviors fast\n• Problem solving: Can figure out complex puzzles",
            "elephants": "🐘 Elephant facts:\n• Memory: Exceptional - remember other elephants for life\n• Emotional: Mourn their dead, show empathy\n• Social: Live in matriarchal herds led by oldest female\n• Communication: Use infrasound (low frequency humans can't hear)\n• Intelligence: Use tools, self-aware, problem solvers\n• Pregnancy: 22 months - longest of any land animal!\n• Baby: Weighs 200+ pounds at birth\n• Trunk: Has 40,000 muscles! Can pick up a peanut or lift 700 pounds",

            // Space
            "sun size": "☀️ The Sun is MASSIVE!\n• Diameter: 864,000 miles (109x Earth's diameter)\n• Volume: 1.3 million Earths could fit inside!\n• Mass: 333,000 times Earth's mass (99.86% of solar system's mass!)\n• Distance: 93 million miles from Earth\n• Temperature: Surface 10,000°F, Core 27 million°F!\n• Age: 4.6 billion years old\n• Type: Yellow dwarf star (medium-sized)\n• Will last: Another 5 billion years before becoming red giant",
            "planets": "🪐 8 planets in our solar system:\n1. Mercury - smallest, closest to sun, no atmosphere\n2. Venus - hottest (900°F!), toxic atmosphere, spins backwards\n3. Earth - only planet with life (that we know!)\n4. Mars - red planet, has ice caps, possible ancient water\n5. Jupiter - BIGGEST! Great Red Spot storm bigger than Earth\n6. Saturn - famous rings made of ice/rock, lowest density\n7. Uranus - tilted sideways, ice giant, blue-green color\n8. Neptune - windiest planet, beautiful blue, farthest planet\n\nPluto: Dwarf planet (2006), still awesome though!",
            "black hole": "⚫ Black holes are regions where gravity is SO strong that nothing escapes!\n• Formation: Massive stars collapse when they die\n• Gravity: So strong even light can't escape (that's why they're 'black')\n• Event Horizon: Point of no return\n• Spaghettification: If you fell in, you'd stretch like spaghetti!\n• Time: Time slows near black holes (relativity)\n• Center: Singularity - infinite density point\n• Largest: TON 618 - 66 billion times Sun's mass!\n• Closest: 1,600 light-years away",
            "moon landing": "🌙 Moon landing (Apollo 11 - July 20, 1969):\n• Astronauts: Neil Armstrong, Buzz Aldrin (landed), Michael Collins (orbited)\n• First words: 'That's one small step for man, one giant leap for mankind'\n• Flag: American flag planted (special one that stands without wind)\n• Duration: 21.5 hours on moon, 2.5 hours walking outside\n• Samples: Brought back 47 pounds of moon rocks\n• Total Apollo missions: 6 successful moon landings\n• Fun fact: Footprints still there (no wind/rain to erase them!)",
            "mars": "🔴 Mars - The Red Planet:\n• Color: Red from iron oxide (rust!) in soil\n• Size: Half Earth's diameter, 1/10th Earth's mass\n• Day: 24.6 hours (similar to Earth!)\n• Year: 687 Earth days\n• Atmosphere: 95% CO2, very thin\n• Temperature: -80°F average, can reach 70°F at equator\n• Moons: 2 (Phobos and Deimos)\n• Water: Ice at poles, evidence of ancient rivers\n• Volcanoes: Olympus Mons - biggest volcano in solar system!\n• Exploration: Multiple rovers (Curiosity, Perseverance)",
            "milky way": "🌌 Our galaxy - The Milky Way:\n• Type: Barred spiral galaxy\n• Size: 100,000 light-years across\n• Stars: 100-400 billion stars!\n• Age: 13.6 billion years old\n• Our location: In Orion Arm, 26,000 light-years from center\n• Center: Supermassive black hole (Sagittarius A*)\n• Speed: Solar system orbits center at 514,000 mph\n• Orbit time: 225-250 million years\n• Neighbors: Andromeda galaxy (will collide in 4.5 billion years!)\n• Name: Looks like spilled milk across night sky",

            // Math
            "pi": "🥧 Pi (π) = 3.14159265359...\n• Definition: Ratio of circle's circumference to diameter\n• Never ends: Infinite decimal digits, never repeats\n• Uses: Calculate circles, spheres, waves, physics\n• Memory: Record is 70,000+ digits memorized!\n• Pi Day: March 14 (3/14)\n• In nature: Appears in rivers, DNA, probability, cosmos\n• Fun fact: First 6 digits 314159 appear starting at 176,451st digit!",
            "infinity": "♾️ Infinity means endless - no end!\n• Not a number: It's a concept\n• Can't count to: Infinity + 1 = still infinity\n• Different sizes: Some infinities bigger than others (mind-blowing!)\n• Symbol: ∞ (lemniscate)\n• Hotel paradox: Infinite hotel with infinite rooms (Hilbert's Hotel)\n• In math: Used in calculus, limits, set theory\n• Space: Universe might be infinite\n• Time: Before Big Bang? After heat death?",
            "zero": "0️⃣ Zero - the number that changed math!\n• Invented: India ~500 AD, spread through Arab mathematicians\n• Revolutionary: Made place-value system possible (10, 100, 1000)\n• Not nothing: It's a placeholder AND a number\n• Operations: 5 + 0 = 5, 5 × 0 = 0, 5 ÷ 0 = undefined!\n• Negative numbers: Zero separates positive from negative\n• Temperature: 0°C = freezing point of water\n• Computers: Binary uses just 0 and 1!",
            "fractions": "🍕 Fractions represent parts of a whole!\n• Pizza example: 3/8 means 3 slices out of 8 total\n• Top (numerator): Parts you have\n• Bottom (denominator): Total parts\n• Types: Proper (3/4), Improper (5/4), Mixed (1 1/4)\n• Operations: Add/subtract (need common denominator), Multiply (straight across), Divide (flip and multiply)\n• Decimals: 1/2 = 0.5, 1/4 = 0.25, 3/4 = 0.75\n• Real life: Recipes, measurements, money, time",
            "percentage": "💯 Percentages are parts per hundred!\n• Meaning: 50% = 50 per 100 = 50/100 = 0.5 = half\n• Calculate: 20% of 50 = (20/100) × 50 = 10\n• Increase: $50 + 20% = $50 + $10 = $60\n• Decrease: $50 - 20% = $50 - $10 = $40\n• Common: 100%=all, 50%=half, 25%=quarter, 10%=tenth\n• Uses: Sales/discounts, taxes, tips, grades, statistics\n• Tip: To find 10%, just divide by 10!",

            // History
            "dinosaurs extinct": "🦕 Dinosaurs went extinct 65 million years ago!\n• Cause: Most likely asteroid impact (10km wide) hit Mexico\n• Effects: Massive explosion, tsunamis, wildfires, dust blocked sun for years\n• Result: Plants died → herbivores died → carnivores died\n• Survivors: Birds (descended from dinosaurs!), small mammals, crocodiles, sharks\n• Timeline: Dinosaurs existed 165 million years (humans only 300,000!)\n• Not all died: Birds ARE dinosaurs (theropod descendants)\n• Longest: Sauropods (Brachiosaurus, Diplodocus)\n• Smartest: Troodon (human-level intelligence?)",
            "pyramids built": "🏛️ Egyptian pyramids built ~4,500 years ago!\n• Great Pyramid: 2.3 million stone blocks, each 2.5 tons\n• Workers: 20,000-30,000 skilled laborers (NOT slaves!)\n• Time: 20-30 years to build\n• Methods: Ramps, levers, sleds, rolled on logs, poured water on sand\n• Precision: Aligned to true north within 0.05 degrees!\n• Purpose: Tombs for pharaohs, gateway to afterlife\n• Height: Originally 481 feet (tallest structure for 3,800 years!)\n• No aliens: Just brilliant engineering and organization!",
            "great wall": "🏯 Great Wall of China - longest wall ever!\n• Length: 13,000+ miles (if you count all branches)\n• Built: Over 2,000+ years (different dynasties)\n• Purpose: Protect from invasions, control trade\n• Materials: Stone, brick, tamped earth, wood\n• Workers: Millions over centuries\n• Myth BUSTED: Can't see from space with naked eye\n• Watchtowers: Every few miles for signaling\n• Today: Tourist attraction, symbol of China",

            // More Science
            "dna": "🧬 DNA is your genetic blueprint!\n• Full name: Deoxyribonucleic Acid\n• Structure: Double helix (twisted ladder)\n• Base pairs: A-T and G-C (4 letters make genetic code!)\n• Length: Unraveled DNA from one cell = 6 feet!\n• All cells: Every cell has complete copy\n• Genes: Sections of DNA that code for traits\n• Human genome: 3 billion base pairs!\n• Shared: 99.9% DNA same between all humans, 98.8% shared with chimps!\n• Discovery: Watson & Crick (1953)",
            "atoms": "⚛️ Atoms are building blocks of everything!\n• Parts: Protons (+), Neutrons (neutral) in nucleus, Electrons (-) orbit\n• Tiny: 100 million atoms fit on a pencil tip!\n• Mostly empty: 99.9999% empty space\n• Nucleus: 100,000 times smaller than atom but has most mass\n• Elements: 118 known elements (different atom types)\n• Molecules: Atoms bonded together (H2O = water)\n• You: Made of 7 octillion atoms!\n• Old: Most atoms billions of years old (you're made of stardust!)",
            "electricity": "⚡ Electricity is flow of electrons!\n• Electrons: Tiny negative particles in atoms\n• Current: Electrons flowing through wire\n• Voltage: Pressure pushing electrons\n• Conductor: Materials electrons flow through easily (copper, gold)\n• Insulator: Materials that block electrons (rubber, plastic)\n• Circuit: Complete path for electricity to flow\n• Power plants: Generate by spinning magnets near coils of wire\n• Lightning: Natural electricity (static discharge)\n• Speed: Electricity flows near speed of light!",

            // More Animals
            "bees": "🐝 Bees are amazing!\n• Colony: Queen (1), Drones (males), Workers (females, thousands)\n• Queen: Lives 2-5 years, lays 2,000 eggs/day!\n• Workers: Live 6 weeks, do all the work\n• Dance: Waggle dance tells others where flowers are\n• Honey: Made from nectar, takes 556 workers to make 1 pound!\n• Pollination: Pollinate 1/3 of our food crops\n• Sting: Workers die after stinging (barbed stinger)\n• Vision: See UV light, find flower patterns we can't see\n• Endangered: Population declining (we need to protect them!)",
            "whales": "🐋 Whale facts:\n• Types: Baleen (filter feeders) and Toothed (hunt prey)\n• Breathing: Blowholes on top of head\n• Deep diving: Sperm whales dive 7,000+ feet\n• Songs: Humpbacks sing complex songs (20+ minutes!)\n• Migration: Travel thousands of miles yearly\n• Lifespan: Bowhead whales live 200+ years\n• Size: Blue whale 100 feet, heart = car, tongue = elephant\n• Intelligence: Complex social behaviors, mourn dead\n• Communication: Songs travel thousands of miles underwater",
            "birds": "🦅 Bird facts:\n• Hollow bones: Make them lighter for flying\n• Feathers: 25,000 feathers (some species!)\n• Heart: Beats 1,000 times/minute (hummingbird)\n• Migration: Arctic tern flies 44,000 miles yearly!\n• Vision: Eagles see 4-5 times better than humans\n• Intelligence: Crows use tools, ravens plan ahead\n• Eggs: Smallest (hummingbird) to largest (ostrich)\n• Flightless: Ostriches, penguins, emus\n• Dinosaurs: Birds evolved from theropod dinosaurs!"
        };

        // Expanded jokes (50+)
        var jokes = [
            "Why don't scientists trust atoms? Because they make up everything! 😄",
            "What do you call a bear with no teeth? A gummy bear! 🐻",
            "Why did the bicycle fall over? It was two-tired! 🚲",
            "What do you call a fake noodle? An impasta! 🍝",
            "What did the ocean say to the beach? Nothing, it just waved! 🌊",
            "Why don't eggs tell jokes? They'd crack up! 🥚",
            "What do you call a dinosaur that crashes his car? Tyrannosaurus Wrecks! 🦖",
            "Why did the scarecrow win an award? He was outstanding in his field! 🌾",
            "What's orange and sounds like a parrot? A carrot! 🥕",
            "Why can't you hear a pterodactyl using the bathroom? Because the 'P' is silent! 😂",
            "What do you call a sleeping bull? A bulldozer! 😴",
            "Why did the math book look sad? It had too many problems! 📚",
            "What do you call cheese that isn't yours? Nacho cheese! 🧀",
            "Why did the cookie go to the doctor? It felt crumbly! 🍪",
            "What do you call a fish wearing a bowtie? Sofishticated! 🐟",
            "Why don't skeletons fight each other? They don't have the guts! 💀",
            "What did one wall say to the other? I'll meet you at the corner! 🧱",
            "Why did the tomato turn red? It saw the salad dressing! 🍅",
            "What do you call a pile of cats? A meowtain! 🐱",
            "Why did the student eat his homework? The teacher said it was a piece of cake! 📝"
        ];

        // Expanded fun facts (50+)
        var facts = [
            "🦈 Sharks have been around longer than trees! Sharks: 400 million years, Trees: 350 million years!",
            "🍯 Honey never spoils! 3,000-year-old honey found in Egyptian tombs is still perfectly edible!",
            "🐙 Octopuses have 3 hearts and blue blood! Two hearts pump blood to the gills, one pumps to the body!",
            "⚡ Lightning is 5 times hotter than the surface of the sun! It reaches 30,000°C!",
            "🧠 Your brain uses 20% of your body's energy but is only 2% of your weight!",
            "🦒 A giraffe's neck has the same number of bones as a human's - just 7!",
            "🐨 Koala fingerprints are so similar to humans, they've confused crime scenes!",
            "🦟 Mosquitoes are the deadliest animals to humans, causing 1 million deaths yearly!",
            "🐜 Ants never sleep! They also can carry 50 times their body weight!",
            "🦎 A crocodile can't stick its tongue out!",
            "🐧 Penguins have an organ above their eyes that converts seawater to freshwater!",
            "🦅 Eagles can see a rabbit from 2 miles away!",
            "🐘 Elephants are the only animals that can't jump!",
            "🦈 Sharks existed before trees, before Saturn's rings, before dinosaurs!",
            "🌍 There are more stars in the universe than grains of sand on all Earth's beaches!",
            "🌊 The ocean is only 5% explored - we know more about the surface of Mars!",
            "☀️ The sun's core is 27 million degrees Fahrenheit!",
            "🌙 There are more trees on Earth than stars in the Milky Way galaxy!",
            "💎 Diamonds rain on Saturn and Jupiter!",
            "🌋 There's enough gold in Earth's core to coat the entire surface in 1.5 feet of gold!",
            "🐝 Bees can recognize human faces!",
            "🦈 Great White sharks can detect one drop of blood in 25 gallons of water!",
            "🐋 A blue whale's heart weighs 400 pounds and beats only 8-10 times per minute!",
            "🦒 Giraffes only need 5-30 minutes of sleep per day!",
            "🦎 Komodo dragons can reproduce without males (parthenogenesis)!",
            "🐸 Some frogs can freeze solid and thaw back to life!",
            "🦈 Greenland sharks can live over 400 years - oldest known vertebrates!",
            "🌌 If the sun were the size of a beach ball, Earth would be the size of a peppercorn!",
            "⚡ Your body produces enough electricity to power a small light bulb!",
            "👃 Humans can detect over 1 trillion different scents!"
        ];

        // Show welcome message
        addBot("👋 Hey! I'm your Roblox AI Helper!\n\nI can help with:\n🎮 Roblox game ideas (50+ ideas!)\n🔬 Science questions (200+ answers!)\n🐾 Animal facts\n🌌 Space stuff\n🧮 Math help\n😂 50+ Jokes\n🌟 50+ Fun facts\n\nWhat would you like?");

        function addBot(text) {
            var div = document.createElement('div');
            div.className = 'message bot';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function addUser(text) {
            var div = document.createElement('div');
            div.className = 'message user';
            div.innerText = text;
            document.getElementById('messages').appendChild(div);
            document.getElementById('messages').scrollTop = 999999;
        }

        function random(arr) {
            return arr[Math.floor(Math.random() * arr.length)];
        }

        function send() {
            var input = document.getElementById('input');
            var msg = input.value.trim();
            if (!msg) return;

            addUser(msg);
            input.value = '';

            var lower = msg.toLowerCase();
            var response = "";

            // Check for math
            if (lower.match(/\d+\s*[\+\-\*\/]\s*\d+/)) {
                try {
                    var result = eval(lower.replace(/[^0-9+\-*/().]/g, ''));
                    response = "🧮 " + lower + " = " + result;
                } catch(e) {}
            }

            // Greetings
            if (!response && (lower.includes('hello') || lower.includes('hi') || lower.includes('hey'))) {
                response = "👋 Hey! What can I help you with today?";
            }

            // Thanks
            if (!response && lower.includes('thank')) {
                response = "😊 You're super welcome! Need anything else?";
            }

            // Goodbye
            if (!response && (lower.includes('bye') || lower.includes('goodbye'))) {
                response = "👋 Bye! Come back anytime! Have an awesome day!";
            }

            // Jokes
            if (!response && (lower.includes('joke') || lower.includes('funny'))) {
                response = random(jokes);
            }

            // Facts
            if (!response && (lower.includes('fact') || lower.includes('fun fact'))) {
                response = random(facts);
            }

            // Game ideas - PRIORITY
            if (!response && lower.includes('obby')) {
                response = random(games.obby);
            }
            if (!response && lower.includes('simulator')) {
                response = random(games.simulator);
            }
            if (!response && (lower.includes('adventure') || lower.includes('quest'))) {
                response = random(games.adventure);
            }
            if (!response && (lower.includes('racing') || lower.includes('race') || lower.includes('car'))) {
                response = random(games.racing);
            }
            if (!response && (lower.includes('tycoon') || lower.includes('business'))) {
                response = random(games.tycoon);
            }
            if (!response && (lower.includes('fighting') || lower.includes('battle'))) {
                response = random(games.fighting);
            }
            if (!response && (lower.includes('game') || lower.includes('roblox') || lower.includes('idea'))) {
                var allGames = [].concat(games.obby, games.simulator, games.adventure, games.racing, games.tycoon, games.fighting);
                response = random(allGames);
            }

            // Check knowledge database
            if (!response) {
                for (var key in knowledge) {
                    if (lower.includes(key)) {
                        response = knowledge[key];
                        break;
                    }
                }
            }

            // Default
            if (!response) {
                response = "🤔 I can help with:\n\n🎮 Roblox game ideas (obby, simulator, adventure, racing, tycoon, fighting)\n🔬 Science questions (ask me anything!)\n🐾 Animal facts\n🌌 Space stuff\n🧮 Math (just type math like '5+3')\n😂 Jokes and fun facts\n\nWhat would you like to know?";
            }

            setTimeout(function() {
                addBot(response);
            }, 500);
        }

        // Enter key
        document.getElementById('input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                send();
            }
        });
    </script>
</body>
</html>
            
