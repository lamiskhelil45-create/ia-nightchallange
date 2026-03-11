# ia-nightchallange
#AI for Daily Life Transformation
import tkinter as tk
from tkinter import font as tkfont
import datetime

# ──────────────────────────────────────────────
#  AI LOGIC  –  keyword-based plan generator
# ──────────────────────────────────────────────
def generate_plan(text):
    text_lower = text.lower()

    sections = []

    # ── EXAM ──────────────────────────────────
    if "exam" in text_lower:
        sections.append(("📌  Priority Tasks", [
            "Study 2 hours (focused session)",
            "Review important lessons",
            "Re-read notes & summaries",
        ]))
        sections.append(("💡  Advice", [
            "Avoid distractions – put phone away",
            "Focus on key exercises",
            "Use the Pomodoro method (25 min on / 5 min off)",
        ]))
        sections.append(("🕐  Schedule", [
            f"{_t(0)}  Deep study session  (60 min)",
            f"{_t(1)}  Short break  (10 min)",
            f"{_t(2)}  Review & practice  (45 min)",
            f"{_t(3)}  Final revision  (30 min)",
        ]))

    # ── TIRED ─────────────────────────────────
    if "tired" in text_lower:
        sections.append(("💡  Advice", [
            "Take a 20-minute power nap",
            "Drink a full glass of water",
            "Sleep early tonight – before 22:00",
            "Avoid screens 30 min before bed",
        ]))

    # ── STRESS ────────────────────────────────
    if "stress" in text_lower or "stressed" in text_lower:
        sections.append(("🧘  Stress Relief", [
            "Take 5 deep breaths right now",
            "Relax for 10 minutes – no tasks",
            "Go for a short 15-min walk outside",
            "Write down what worries you most",
        ]))

    # ── HOMEWORK ──────────────────────────────
    if "homework" in text_lower:
        sections.append(("📌  Priority Tasks", [
            "Finish all homework first",
            "Check answers carefully",
            "Organize completed work",
        ]))

    # ── DEFAULT (no keyword matched) ──────────
    if not sections:
        sections.append(("🚀  Smart Plan", [
            "Start with your most important task",
            "Break big tasks into small steps",
            "Take a 5-min break every 45 minutes",
            "Review your progress at day end",
        ]))
        sections.append(("💡  General Advice", [
            "Stay hydrated – drink water regularly",
            "Keep your workspace clean & tidy",
            "Avoid multitasking – focus on one thing",
        ]))

    return sections


def _t(offset_hours):
    """Return a time string offset from the current hour."""
    now = datetime.datetime.now()
    future = now + datetime.timedelta(hours=offset_hours)
    return future.strftime("%H:%M")


# ──────────────────────────────────────────────
#  BUILD UI
# ──────────────────────────────────────────────
class LifePilotApp:
    # ── Palette ───────────────────────────────
    BG         = "#F7F8FC"
    CARD_BG    = "#FFFFFF"
    ACCENT     = "#4F6EF7"        # indigo-blue
    ACCENT2    = "#7C3AED"        # violet
    TEXT_DARK  = "#1E1E2E"
    TEXT_MID   = "#5B5C7A"
    TEXT_LIGHT = "#9394B0"
    SUCCESS    = "#22C55E"
    BORDER     = "#E4E6F0"

    def __init__(self, root):
        self.root = root
        self._configure_window()
        self._load_fonts()
        self._build_ui()

    # ── Window setup ──────────────────────────
    def _configure_window(self):
        self.root.title("LifePilot AI")
        self.root.geometry("500x700")
        self.root.resizable(False, False)
        self.root.configure(bg=self.BG)
        # Center on screen
        self.root.update_idletasks()
        x = (self.root.winfo_screenwidth()  - 500) // 2
        y = (self.root.winfo_screenheight() - 700) // 2
        self.root.geometry(f"500x700+{x}+{y}")

    # ── Font helpers ──────────────────────────
    def _load_fonts(self):
        self.font_title   = tkfont.Font(family="Helvetica Neue", size=26, weight="bold")
        self.font_sub     = tkfont.Font(family="Helvetica Neue", size=11)
        self.font_label   = tkfont.Font(family="Helvetica Neue", size=12, weight="bold")
        self.font_input   = tkfont.Font(family="Helvetica Neue", size=12)
        self.font_btn     = tkfont.Font(family="Helvetica Neue", size=13, weight="bold")
        self.font_section = tkfont.Font(family="Helvetica Neue", size=12, weight="bold")
        self.font_item    = tkfont.Font(family="Helvetica Neue", size=11)
        self.font_footer  = tkfont.Font(family="Helvetica Neue", size=9)

    # ──────────────────────────────────────────
    #  FULL UI BUILD
    # ──────────────────────────────────────────
    def _build_ui(self):
        # ── Outer wrapper ─────────────────────
        wrapper = tk.Frame(self.root, bg=self.BG)
        wrapper.pack(fill="both", expand=True, padx=20, pady=16)

        # ── HEADER ────────────────────────────
        header = tk.Frame(wrapper, bg=self.BG)
        header.pack(fill="x", pady=(0, 12))

        # Gradient pill badge
        badge = tk.Label(
            header, text="  AI PLANNER  ",
            bg=self.ACCENT, fg="white",
            font=tkfont.Font(family="Helvetica Neue", size=9, weight="bold"),
            padx=8, pady=3
        )
        badge.pack()

        tk.Label(
            header, text="LifePilot AI 🤖",
            font=self.font_title,
            bg=self.BG, fg=self.TEXT_DARK
        ).pack(pady=(6, 2))

        tk.Label(
            header, text="Your personal AI daily planner",
            font=self.font_sub,
            bg=self.BG, fg=self.TEXT_MID
        ).pack()

        # ── INPUT CARD ────────────────────────
        in_card = self._card(wrapper)
        in_card.pack(fill="x", pady=(0, 10))

        tk.Label(
            in_card, text="What is your plan today?",
            font=self.font_label,
            bg=self.CARD_BG, fg=self.TEXT_DARK,
            anchor="w"
        ).pack(fill="x", padx=16, pady=(14, 6))

        # Input text area with custom border frame
        input_frame = tk.Frame(in_card, bg=self.BORDER, bd=0)
        input_frame.pack(fill="x", padx=16, pady=(0, 14))

        self.input_box = tk.Text(
            input_frame,
            height=4,
            font=self.font_input,
            bg="#F1F2FB",
            fg=self.TEXT_DARK,
            insertbackground=self.ACCENT,
            relief="flat",
            padx=12, pady=10,
            wrap="word",
            bd=0
        )
        self.input_box.pack(fill="x", padx=1, pady=1)

        # Placeholder
        self._set_placeholder()

        # ── BUTTON ────────────────────────────
        btn_frame = tk.Frame(wrapper, bg=self.BG)
        btn_frame.pack(fill="x", pady=(0, 12))

        self.btn = tk.Button(
            btn_frame,
            text="✨  Generate Smart Plan",
            font=self.font_btn,
            bg=self.ACCENT,
            fg="white",
            activebackground=self.ACCENT2,
            activeforeground="white",
            relief="flat",
            cursor="hand2",
            padx=24, pady=12,
            bd=0,
            command=self._on_generate
        )
        self.btn.pack(fill="x")

        # Hover effects
        self.btn.bind("<Enter>", lambda e: self.btn.config(bg=self.ACCENT2))
        self.btn.bind("<Leave>", lambda e: self.btn.config(bg=self.ACCENT))

        # ── OUTPUT CARD ───────────────────────
        out_card = self._card(wrapper)
        out_card.pack(fill="both", expand=True)

        out_header = tk.Frame(out_card, bg=self.CARD_BG)
        out_header.pack(fill="x", padx=16, pady=(14, 8))

        tk.Label(
            out_header, text="📋  Your AI Plan",
            font=self.font_label,
            bg=self.CARD_BG, fg=self.TEXT_DARK
        ).pack(side="left")

        self.status_dot = tk.Label(
            out_header, text="●",
            font=tkfont.Font(size=10),
            bg=self.CARD_BG, fg=self.TEXT_LIGHT
        )
        self.status_dot.pack(side="right")

        # Scrollable output
        scroll_frame = tk.Frame(out_card, bg=self.CARD_BG)
        scroll_frame.pack(fill="both", expand=True, padx=16, pady=(0, 14))

        scrollbar = tk.Scrollbar(scroll_frame, orient="vertical", width=6)
        scrollbar.pack(side="right", fill="y")

        self.output_box = tk.Text(
            scroll_frame,
            font=self.font_item,
            bg=self.CARD_BG,
            fg=self.TEXT_DARK,
            relief="flat",
            padx=4, pady=4,
            wrap="word",
            state="disabled",
            bd=0,
            yscrollcommand=scrollbar.set,
            cursor="arrow",
            spacing1=3, spacing2=2, spacing3=3
        )
        self.output_box.pack(side="left", fill="both", expand=True)
        scrollbar.config(command=self.output_box.yview)

        # Configure text tags for rich formatting
        self.output_box.tag_config("section",  font=self.font_section,
                                    foreground=self.ACCENT,   spacing1=10, spacing3=4)
        self.output_box.tag_config("item",     font=self.font_item,
                                    foreground=self.TEXT_DARK, lmargin1=12, lmargin2=20)
        self.output_box.tag_config("hint",     font=self.font_item,
                                    foreground=self.TEXT_MID,  justify="center")
        self.output_box.tag_config("divider",  font=self.font_footer,
                                    foreground=self.BORDER)

        # Initial hint
        self._show_hint("Type your plans or feelings above,\nthen tap Generate Smart Plan ✨")

        # ── FOOTER ────────────────────────────
        tk.Label(
            wrapper, text="Powered by LifePilot AI",
            font=self.font_footer,
            bg=self.BG, fg=self.TEXT_LIGHT
        ).pack(pady=(8, 0))

    # ──────────────────────────────────────────
    #  HELPERS
    # ──────────────────────────────────────────
    def _card(self, parent):
        """Return a white rounded-look Frame."""
        return tk.Frame(
            parent,
            bg=self.CARD_BG,
            relief="flat",
            bd=1,
            highlightthickness=1,
            highlightbackground=self.BORDER,
            highlightcolor=self.ACCENT
        )

    # ── Placeholder text logic ─────────────────
    def _set_placeholder(self):
        self._placeholder_active = True
        self.input_box.insert("1.0", "e.g. I have exam tomorrow and I feel stressed…")
        self.input_box.config(fg=self.TEXT_LIGHT)
        self.input_box.bind("<FocusIn>",  self._clear_placeholder)
        self.input_box.bind("<FocusOut>", self._restore_placeholder)

    def _clear_placeholder(self, event=None):
        if self._placeholder_active:
            self.input_box.delete("1.0", "end")
            self.input_box.config(fg=self.TEXT_DARK)
            self._placeholder_active = False

    def _restore_placeholder(self, event=None):
        if not self.input_box.get("1.0", "end").strip():
            self._placeholder_active = True
            self.input_box.insert("1.0", "e.g. I have exam tomorrow and I feel stressed…")
            self.input_box.config(fg=self.TEXT_LIGHT)

    # ── Output helpers ────────────────────────
    def _show_hint(self, msg):
        self.output_box.config(state="normal")
        self.output_box.delete("1.0", "end")
        self.output_box.insert("end", "\n" + msg, "hint")
        self.output_box.config(state="disabled")

    def _write_plan(self, sections):
        self.output_box.config(state="normal")
        self.output_box.delete("1.0", "end")

        for i, (title, items) in enumerate(sections):
            if i > 0:
                self.output_box.insert("end", "\n──────────────────────\n", "divider")
            self.output_box.insert("end", title + "\n", "section")
            for item in items:
                self.output_box.insert("end", f"  •  {item}\n", "item")

        self.output_box.config(state="disabled")

    # ──────────────────────────────────────────
    #  MAIN ACTION
    # ──────────────────────────────────────────
    def _on_generate(self):
        raw = self.input_box.get("1.0", "end").strip()

        # Ignore placeholder or empty
        if not raw or self._placeholder_active or raw.startswith("e.g."):
            self._show_hint("Please describe your plans or how you feel 🙂")
            return

        # Show "thinking" state
        self.btn.config(text="⏳  Analysing…", state="disabled")
        self.status_dot.config(fg="#FACC15")  # yellow = processing
        self.root.update()

        # Generate plan
        sections = generate_plan(raw)

        # Render output
        self._write_plan(sections)
        self.status_dot.config(fg=self.SUCCESS)  # green = done

        # Restore button
        self.btn.config(text="✨  Generate Smart Plan", state="normal")


# ──────────────────────────────────────────────
#  ENTRY POINT
# ──────────────────────────────────────────────
if __name__ == "__main__":
    root = tk.Tk()
    app  = LifePilotApp(root)
    root.mainloop()
