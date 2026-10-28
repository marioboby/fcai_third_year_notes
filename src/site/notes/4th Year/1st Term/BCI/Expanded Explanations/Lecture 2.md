---
{"dg-publish":true,"permalink":"/4th-year/1st-term/bci/expanded-explanations/lecture-2/","dg-note-properties":{}}
---

# Intuitive Guide to EEG Montages and Re-Referencing

> [!Abstract] 
> An EEG electrode can never measure the "absolute" electrical voltage of a single spot on the brain because voltage is fundamentally a _comparison_ between two points. A **Montage** is simply the rulebook we choose for _which_ two points we compare to create our signal channels, and **Re-Referencing** is the simple subtraction trick we use on the computer after recording to change our point of comparison and clean up our view of the brain.

  

Don't worry at all—this is genuinely the #1 concept that trips up almost every single student in their first BCI course! The reason it feels confusing is that we naturally assume an EEG sensor works like a thermometer: you put it on spot $X$, and it tells you the temperature at spot $X$. **Electricity doesn't work like that.** Let’s strip away the heavy math and build your intuition from the ground up using a simple real-world mental model.

  

## 1. The Core Problem: Why Do We Need a "Reference" at All?

### 1.1 The "Mountain Altitude" Rule of Electricity

- **Electrical Potential (Voltage)** is strictly a measurement of the _difference_ in electrical charge between two locations ($Point\ A - Point\ B$). A single wire touching a single spot on the head measures nothing on its own.
    
      
    
- **Analogy:** Imagine I ask you, _"How high up is the top of Cairo Tower?"_ You can't answer that with a single point in space—you have to measure the distance between the top of the tower and a **Reference Point** (like **Sea Level**, or the **Street Level** right outside).
    
      
    - **Active Electrode (Sensor):** The spot on the scalp you actually care about (the top of the tower).
        
          
        
    - **Reference Electrode:** The baseline spot you compare it against (Sea Level).
        
          
        
    - **Channel:** The final number your computer saves, which is always:
        
          
        
        $$\text{Channel} = \text{Active Electrode} - \text{Reference Electrode}$$
        

### 1.2 The Catch: The Human Head Has No "Sea Level"

- Ideally, we want to put our **Reference Electrode** somewhere on the head that has $0\text{ Volts}$ of brain activity (an electrically "quiet" spot) so that $\text{Active} - 0 = \text{Active}$.
    
      
    
- **The Fundamental Problem:** Every single millimeter of your head is conductive and carries electrical noise (either from the brain underneath, your heartbeat, or your jaw/neck muscles). There is no true "zero" spot on the human body!
    
      
    
- Because our hardware reference isn't perfect during the live recording, we use **Re-Referencing** (simple addition and subtraction in our code afterward) to mathematically swap out that reference for a better one.
    
      
    

## 2. What is a Montage vs. Re-Referencing?

- **Montage:** Think of a **Montage** as a **camera lens or viewing layout**. If you have $32\text{ electrodes}$ on a cap, how do you pair them up to display channels on your screen? Do you compare every electrode to the top of the head? To the ears? Or to the electrode right next to it? Each pairing recipe is called a **Montage**.
    
      
    
- **Re-Referencing:** Because every channel is just basic subtraction ($A - B$), we can change the reference _after_ the experiment is over without ever touching the physical wires on the person's head!
    
      
    - **How the "Magic Trick" Works:**
        
          
        - Suppose during the experiment, you measured Electrode $A$ and Electrode $B$ using the top of the head ($Cz$) as your hardware reference:
            
              
            
            $$\text{Channel } A = V_A - V_{Cz}$$
            
            $$\text{Channel } B = V_B - V_{Cz}$$
            
        - What if, after the experiment, you want to know the difference directly between $A$ and $B$, completely getting rid of $Cz$? You just subtract Channel $B$ from Channel $A$ in Python/MATLAB:
            
              
            
            $$\text{Channel } A - \text{Channel } B = (V_A - V_{Cz}) - (V_B - V_{Cz}) = V_A - V_B$$
            
        - Look at that: **$V_{Cz}$ completely vanished!** That simple subtraction in software is all **Re-Referencing** is.
            
              
            

## 3. The Four Montages from Lecture (Explained Simply)

To make this concrete, imagine our scalp electrodes are having a conversation in a crowded room, and at one specific spot ($C_3$, over the left motor cortex), a neuron group shouts **"HELLO!"** ($+10\text{ }\mu\text{V}$), while the whole room has a loud air-conditioner hum of **$+50\text{ }\mu\text{V}$** everywhere. Let's see how the four montages on Slide 2 handle this:

  

### 3.1 Reference Montage (Common Reference: $Cz$ or Mastoids)

- **How it works:** We pick **one physical spot** on the head to be the "boss" reference, and compare every single other electrode to that one spot.
    
      
    - **$Cz$ Reference:** Uses the exact center/top of the head ($Cz$) as the reference for everyone else.
        
          
        
    - **Mastoid Reference:** Uses the bony bump right behind your ear (**Mastoid**) as the reference because there is thick bone and very little brain tissue there.
        
          
        
- **The Good:** Super simple to understand. Every channel is just $\text{Electrode}_i - V_{Cz}$.
    
      
    
- **The Bad (The "Blind Spot" & "Ghost" Problem):**
    
      
    - **Blind Spot:** Electrodes physically right next to $Cz$ have almost the exact same voltage as $Cz$. When you subtract $Cz$ from them, they cancel out to nearly zero! You become "blind" to brain activity near the top of the head.
        
          
        
    - **Ghost Signal:** If something huge happens right under $Cz$ (say, a $+20\text{ }\mu\text{V}$ spike), you end up subtracting $+20\text{ }\mu\text{V}$ from _every single electrode on the head_, making it look like the entire brain just had a $-20\text{ }\mu\text{V}$ drop!
        
          
        
- **Analogy:** Measuring everyone's height in a classroom by comparing them to **Ahmed** ($Cz$). Anyone standing right next to Ahmed of the same height looks like they have a height of $0\text{ cm}$ (Blind Spot). And if Ahmed suddenly jumps up on a chair, the math makes it look like the entire classroom suddenly sank into the floor (Ghost Signal)!
    
      
    

### 3.2 Bipolar Montage (Chain of Neighbors)

- **How it works:** Instead of comparing everyone to one central boss, electrodes are paired up in a daisy-chain line from front to back (or left to right):
    
      
    - $\text{Channel } 1 = \text{Electrode } 1 - \text{Electrode } 2$
        
          
        
    - $\text{Channel } 2 = \text{Electrode } 2 - \text{Electrode } 3$
        
          
        
    - $\text{Channel } 3 = \text{Electrode } 3 - \text{Electrode } 4$
        
          
        
- **The Good:** Doctors love this for spotting epilepsy seizures because if a spike happens right at **Electrode 2**, then $\text{Channel } 1$ ($E_1 - E_2$) goes **down** (negative) while $\text{Channel } 2$ ($E_2 - E_3$) goes **up** (positive). The two wave lines point right at each other on the screen like an arrow saying _"The problem is right here at Electrode 2!"_
    
      
    
- **The Bad:** If a slow, wide brain wave covers both Electrode 1 and Electrode 2 equally, subtracting them gives zero ($10 - 10 = 0$), so you miss big, widespread brain waves.
    
      
    
- **Analogy:** Instead of measuring height from sea level, you walk up a staircase and only measure the height difference **from one step to the very next step**. You can instantly spot if one step is broken or extra tall, but it’s hard to tell how high up in the building you are overall.
    
      
    

### 3.3 Common Average Reference (CAR)

- **How it works:** Instead of trusting a single physical electrode like $Cz$ (which might be sitting right over active brain tissue), we calculate the **average voltage of all $32\text{ electrodes}$ combined** at that exact millisecond, and subtract that global average from each electrode:
    
      
    
    $$\text{New Channel } i = \text{Old Channel } i - \text{Average of All 32 Channels}$$
    
- **Why does this work?**
    
      
    - At any given millisecond, some parts of the head are slightly positive ($+$) and other parts are slightly negative ($-$), so genuine local brain waves average out to roughly $0$.
        
          
        
    - Meanwhile, any **global background noise** that hits the entire head at once (like the $+50\text{ }\mu\text{V}$ room hum or a bad hardware reference) is present in _all_ $32\text{ channels}$, so it shows up in the average and gets subtracted away cleanly!
        
          
        
- **The Catch:** If one electrode wire breaks and starts screaming with $+1000\text{ }\mu\text{V}$ of static noise, including it in the average will ruin all $31$ other clean channels! You must drop bad channels before calculating CAR.
    
      
    
- **Analogy:** Imagine recording a singer on stage, but there is a loud air conditioner humming in the room. If you place $32\text{ microphones}$ all over the auditorium, average all $32$ recordings together (which captures the room-wide AC hum), and subtract that average from the singer's microphone, the AC hum disappears and the singer's voice comes through crystal clear!
    
      
    

### 3.4 Laplacian Montage (The "Local Spotlight")

- **How it works:** Instead of subtracting the average of the _entire head_ (like CAR), for each electrode, you only subtract the average of its **4 immediate surrounding neighbors** (Front, Back, Left, Right):
    
      
    
    $$\text{New Channel } C_3 = \text{Old Channel } C_3 - \text{Average of } (F_3, P_3, Cz, T_7)$$
    
- **Why does this work?**
    
      
    - Remember earlier when we talked about how the skull blurs electricity (**Volume Conduction**), so a signal from deep in the brain spreads out smoothly across many neighboring electrodes?
        
          
        
    - Because neighboring electrodes share that blurry spread-out signal, subtracting the $4$ immediate neighbors **erases all blurry, spread-out background activity** and acts like a laser spotlight on ONLY the sharp brain activity happening **directly underneath that one center electrode**!
        
          
        
- **Analogy:** Since you've worked with image processing kernels before, think of the **Laplacian montage** as literally applying that **4-neighbor sharpening kernel** to the scalp! By taking the center pixel (electrode) and subtracting the $4$ immediate neighbors around it, you erase smooth blurry gradients and make sharp local edges pop right out.
    
      
    

## Summary Cheat-Sheet for Your Obsidian Notes

|**Montage / Reference**|**Simple Formula**|**Best Used When...**|**Main Weakness**|
|---|---|---|---|
|**Reference ($Cz$ / Mastoid)**|$\text{Ch}_i = V_i - V_{\text{ref}}$|Raw hardware recording; viewing broad waves far from the reference site (e.g., P300 with Mastoid ref).|Creates a "blind spot" near the reference electrode; if the reference site is noisy, _every_ channel gets polluted.|
|**Bipolar**|$\text{Ch}_i = V_i - V_{i+1}$|Finding the exact spot of a sharp local spike (clinical epilepsy) or measuring EOG eye movements.|Erases wide, smooth brain waves shared by two neighboring sensors.|
|**Common Average (CAR)**|$\text{Ch}_i = V_i - \text{Mean}(\text{All } N \text{ sensors})$|You have $32+$ electrodes covering the whole head and want to remove global room/reference noise.|One broken/noisy electrode ruins the average for the entire dataset.|
|**Laplacian**|$\text{Ch}_i = V_i - \text{Mean}(4 \text{ closest neighbors})$|Isolating pinpoint motor cortex activity (e.g., Left vs. Right hand motor imagery at $C_3 / C_4$).|Erases deep/broad brain waves; doesn't work well on outer edge electrodes that don't have $4$ neighbors.|

## Professor's Synthesis

If you ever feel lost in an exam question or coding assignment about montages, just ask yourself one question: **"What am I subtracting from my sensor?"**

  

- Subtracting **one fixed sensor ($Cz$/Mastoid)**? That's a **Reference Montage**.
    
      
    
- Subtracting **one neighbor in a chain**? That's **Bipolar**.
    
      
    
- Subtracting the **average of the whole head**? That's **CAR** (cleans global noise).
    
      
    
- Subtracting the **average of the 4 surrounding neighbors**? That's **Laplacian** (sharpens local spots like an image edge filter).
    
      
    

# Worked Numerical Walkthrough: Reference vs. CAR vs. Laplacian

**Conceptual Abstract:** By tracking the exact same millisecond of brain activity through five electrodes using concrete numbers, we can watch how **Common Average Referencing (CAR)** strips away head-wide electrical interference while **Laplacian Re-Referencing** erases blurry regional drift to isolate a single, pinpoint cortical signal.

  

Let’s set up a miniature virtual experiment right now. Grab a mental pen and paper—once you see the arithmetic happen with simple whole numbers, the difference between these montages will click permanently.

  

## 1. The Ground Truth Setup (What is _Actually_ Happening Inside the Head)

Imagine we are looking at a cross-shaped cluster of **5 electrodes** over the left motor cortex ($C_3$ in the middle, surrounded by its 4 neighbors: Front $F_3$, Back $P_3$, Left $T_7$, and Right $Cz$), plus **5 other electrodes** on the opposite side of the head (total $N = 10\text{ electrodes}$ on the cap for easy division).

  

At millisecond $t = 1$, three physical things happen simultaneously:

  

1. **Global Room Noise ($+100\text{ }\mu\text{V}$):** The hospital power line hums, adding $+100\text{ }\mu\text{V}$ to **every single electrode** on the entire head.
    
      
    
2. **Blurry Regional Wave ($+20\text{ }\mu\text{V}$):** A deep, drowsy brain wave spreads equally across the entire **left side** of the head (hitting $C_3$ and all 4 of its neighbors with $+20\text{ }\mu\text{V}$, while the 5 electrodes on the right side of the head get $0\text{ }\mu\text{V}$).
    
      
    
3. **The Target BCI Signal ($+12\text{ }\mu\text{V}$):** The user imagines moving their right hand, causing a tiny, sharp spike of **$+12\text{ }\mu\text{V}$ ONLY directly underneath center electrode $C_3$**!
    
      
    

> **Our Goal:** As BCI engineers, we want our final number for **$C_3$** to be **$+12\text{ }\mu\text{V}$** (the pure hand-movement signal), and we want the surrounding non-motor electrodes to read **$0\text{ }\mu\text{V}$**.
> 
>   

### What the Raw Electrodes Actually Feel:

If we add up those three sources (Global Noise + Blurry Regional Wave + Local Target Signal) at each spot, here is the raw voltage at our 10 electrodes:

  

```
                      [Front: F3]
                       120 uV
                          |
                          |
     [Left: T7] ------ [Center: C3] ------ [Right: Cz]
       120 uV            132 uV              120 uV
                          |
                          |
                      [Back: P3]
                       120 uV

   (Plus 5 electrodes on the opposite side of the head, 
    each feeling only the Global Room Noise = 100 uV each)
```

- **Center Electrode ($C_3$):** $100\text{ (Global)} + 20\text{ (Blurry)} + 12\text{ (Target)} = \mathbf{132\text{ }\mu\text{V}}$
    
      
    
- **The 4 Immediate Neighbors ($F_3, P_3, T_7, Cz$):** $100\text{ (Global)} + 20\text{ (Blurry)} + 0\text{ (Target)} = \mathbf{120\text{ }\mu\text{V}\text{ each}}$
    
      
    
- **The 5 Far-Away Electrodes (Right Hemisphere):** $100\text{ (Global)} + 0\text{ (Blurry)} + 0\text{ (Target)} = \mathbf{100\text{ }\mu\text{V}\text{ each}}$
    
      
    

Now, let's see what happens when we view these exact numbers through our different **Montages**!

  

## 2. Case 1: The $Cz$ Reference Montage (Why Hardware Reference Can Mislead You)

Suppose the amplifier is set to a **$Cz$ Reference Montage** (meaning the hardware subtracts $Cz = 120\text{ }\mu\text{V}$ from every electrode on the head):

  

$$\text{Channel}_i^{\text{Ref}} = V_i - V_{Cz}$$

- **What happens to $C_3$?**
    
      
    
    $$C_3^{\text{Ref}} = 132 - 120 = \mathbf{+12\text{ }\mu\text{V}}$$
    
- **What happens to Neighbor $F_3$?**
    
      
    
    $$F_3^{\text{Ref}} = 120 - 120 = \mathbf{0\text{ }\mu\text{V}}$$
    
- **What happens to the 5 Far-Away Electrodes on the right side?**
    
      
    
    $$\text{Far Electrode}^{\text{Ref}} = 100 - 120 = \mathbf{-20\text{ }\mu\text{V}}$$
    
- **Professor's Diagnosis:** Look at the right side of the head! Nothing happened there at all, yet the computer says all 5 electrodes on the right side just had a **$-20\text{ }\mu\text{V}$ negative brain wave**! Why? Because $Cz$ had the $+20\text{ }\mu\text{V}$ blurry wave sitting under it, and subtracting $Cz$ injected a fake "ghost" $-20\text{ }\mu\text{V}$ signal into the quiet side of the brain. Furthermore, $Cz$ itself is now $120 - 120 = 0\text{ }\mu\text{V}$ (a total blind spot).
    
      
    

## 3. Case 2: Common Average Reference (CAR)

Now let’s apply **CAR**. Remember the rule: **Find the average of ALL 10 electrodes on the whole head, and subtract it from each channel**.

  

- **Step 1: Sum all 10 electrodes on the head:**
    
      
    - One center electrode ($C_3$): $132\text{ }\mu\text{V}$
        
          
        
    - Four neighbors ($4 \times 120\text{ }\mu\text{V}$): $480\text{ }\mu\text{V}$
        
          
        
    - Five far-away electrodes ($5 \times 100\text{ }\mu\text{V}$): $500\text{ }\mu\text{V}$
        
          
        
    - $\text{Total Sum} = 132 + 480 + 500 = 1112\text{ }\mu\text{V}$
        
          
        
- **Step 2: Divide by $N = 10$ to get the Global Average:**
    
      
    
    $$\text{Global Mean} = \frac{1112}{10} = \mathbf{111.2\text{ }\mu\text{V}}$$
    
- **Step 3: Subtract $111.2\text{ }\mu\text{V}$ from our channels:**
    
      
    - **New $C_3^{\text{CAR}}$:** $132 - 111.2 = \mathbf{+20.8\text{ }\mu\text{V}}$
        
          
        
    - **New Neighbor $F_3^{\text{CAR}}$:** $120 - 111.2 = \mathbf{+8.8\text{ }\mu\text{V}}$
        
          
        
    - **New Far Electrode$^{\text{CAR}}$:** $100 - 111.2 = \mathbf{-11.2\text{ }\mu\text{V}}$
        
          
        
- **Professor's Diagnosis:**
    
      
    - **What CAR fixed:** Notice how the massive **$+100\text{ }\mu\text{V}$ Global Room Noise** is completely gone! All numbers are now small, clean microvolt values centered around zero.
        
          
        
    - **What CAR _couldn't_ fix:** Why is $C_3$ reading $+20.8\text{ }\mu\text{V}$ instead of $+12\text{ }\mu\text{V}$, and why is neighbor $F_3$ still reading $+8.8\text{ }\mu\text{V}$? Because **CAR only removes noise that affects the _entire_ head equally**. Since the $+20\text{ }\mu\text{V}$ blurry wave only hit the left half of the head, CAR couldn't erase it!
        
          
        

## 4. Case 3: The Laplacian Montage (The Spatial Sharpening Filter)

Now let’s apply the **Laplacian Montage** to center electrode $C_3$. Remember the rule: **Ignore the rest of the head; only subtract the average of $C_3$'s 4 immediate surrounding neighbors ($F_3, P_3, T_7, Cz$)**.

  

- **Step 1: Average the 4 immediate neighbors around $C_3$:**
    
      
    
    $$\text{Local Neighbor Mean} = \frac{F_3 + P_3 + T_7 + Cz}{4} = \frac{120 + 120 + 120 + 120}{4} = \mathbf{120\text{ }\mu\text{V}}$$
    
- **Step 2: Subtract that local neighbor average from $C_3$:**
    
      
    
    $$C_3^{\text{Lap}} = V_{C_3} - \text{Local Neighbor Mean} = 132 - 120 = \mathbf{+12\text{ }\mu\text{V}}$$
    
- **What if we also apply the Laplacian to a neighbor (assuming its surrounding neighbors also just feel the $120\text{ }\mu\text{V}$ regional wave)?**
    
      
    
    $$F_3^{\text{Lap}} = 120 - 120 = \mathbf{0\text{ }\mu\text{V}}$$
    
- **Professor's Diagnosis:** **Bullseye!** Look at that result: **$+12\text{ }\mu\text{V}$**!
    
      
    - Because all 4 surrounding neighbors felt the $+100\text{ }\mu\text{V}$ Global Room Noise _AND_ all 4 surrounding neighbors felt the $+20\text{ }\mu\text{V}$ Blurry Regional Wave, their local average ($120\text{ }\mu\text{V}$) captured **both sources of background clutter** at once!
        
          
        
    - Subtracting $120$ from $132$ stripped away both the room noise and the blurry regional wave in a single step, leaving **only the $+12\text{ }\mu\text{V}$ hand-movement signal** sitting directly beneath $C_3$.
        
          
        

### Side-by-Side Comparison of Our Numbers

|**Measurement Step**|**Value at Center (C3C_3C3​)**|**Value at Neighbor (F3F_3F3​)**|**Value at Far Right Side**|**What Happened?**|
|---|---|---|---|---|
|**True Target Signal**|**$+12\text{ }\mu\text{V}$**|**$0\text{ }\mu\text{V}$**|**$0\text{ }\mu\text{V}$**|_The hidden truth inside the brain._|
|**Raw Sensor Voltage**|$132\text{ }\mu\text{V}$|$120\text{ }\mu\text{V}$|$100\text{ }\mu\text{V}$|Buried under $+100$ room noise and $+20$ blurry wave.|
|**After CAR**|$+20.8\text{ }\mu\text{V}$|$+8.8\text{ }\mu\text{V}$|$-11.2\text{ }\mu\text{V}$|**Erased the $+100$ global room noise**, but the $+20$ blurry regional wave still smears across the channels.|
|**After Laplacian**|**$+12.0\text{ }\mu\text{V}$**|**$0.0\text{ }\mu\text{V}$**|**$0.0\text{ }\mu\text{V}$**|**Erased BOTH the $+100$ global noise AND the $+20$ blurry regional wave**, recovering the exact $+12\text{ }\mu\text{V}$ target!|

## 5. Wait—If Laplacian is So Good, Why Ever Use CAR?

Looking at the table above, you might naturally ask: _"Professor, why don't we just use the Laplacian montage 100% of the time?!"_

  

- **What if the $+20\text{ }\mu\text{V}$ blurry regional wave WAS the signal you wanted to study?**
    
      
    - For example, the **P300 wave** (used in P300 spellers) or **Emotion waves** are wide, smooth waves that spread across the whole parietal or frontal lobe at once ($C_3$ and all 4 of its neighbors go up by $+20\text{ }\mu\text{V}$ together).
        
          
        
    - If you use **Laplacian** on a wide P300 wave, it subtracts $120 - 120 = 0\text{ }\mu\text{V}$ and **deletes your P300 signal completely**!
        
          
        
    - In that case, **CAR** is the hero: it deletes the $+100\text{ }\mu\text{V}$ room noise while keeping the $+20\text{ }\mu\text{V}$ regional brain wave alive!
        
          
        

## Professor's Synthesis

Here is your ultimate rule of thumb from the numbers we just crunched:

  

- **CAR** subtracts the **whole-head average** ($111.2\text{ }\mu\text{V}$) $\rightarrow$ Deletes **whole-head noise** ($+100\text{ }\mu\text{V}$), keeps **broad regional waves** ($+20\text{ }\mu\text{V}$) AND **local spikes** ($+12\text{ }\mu\text{V}$).
    
      
    
- **Laplacian** subtracts the **4-neighbor average** ($120\text{ }\mu\text{V}$) $\rightarrow$ Deletes **whole-head noise** ($+100\text{ }\mu\text{V}$) AND **broad regional waves** ($+20\text{ }\mu\text{V}$), keeping **ONLY sharp local spikes** ($+12\text{ }\mu\text{V}$).
    
      
    

