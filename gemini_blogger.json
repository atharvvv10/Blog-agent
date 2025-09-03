from flask import Flask, request, jsonify
import google.generativeai as genai
import os

# --------------------------
# CONFIGURE GEMINI API
# --------------------------
# You can either hardcode or use environment variable
# Example: setx GEMINI_API_KEY "your-real-api-key"
API_KEY = os.getenv("GEMINI_API_KEY", "AIzaSyAn8f2qo0BlUy2IUoqn4USBsSx6O4u1E3I")
genai.configure(api_key=API_KEY)

app = Flask(__name__)

# --------------------------
# ROUTE: Generate Blog
# --------------------------
@app.route("/generate_blog", methods=["POST"])
def generate_blog():
    try:
        data = request.get_json(force=True)
        topic = data.get("topic", "Latest Tech News")

        prompt = f"""
        Write a detailed blog post about: {topic}.
        Structure:
        - Title
        - Introduction
        - 4–5 sections with subheadings
        - Conclusion
        Also include SEO keywords + meta description at the end.
        """

        # Try high-quality model first
        try:
            model = genai.GenerativeModel("gemini-1.5-pro")
            response = model.generate_content(prompt)
        except Exception:
            # Fallback to faster model if pro is unavailable
            model = genai.GenerativeModel("gemini-1.5-flash")
            response = model.generate_content(prompt)

        blog_text = response.text if response and response.text else "⚠️ No blog generated"

        return jsonify({"topic": topic, "blog": blog_text})

    except Exception as e:
        return jsonify({"error": str(e)}), 500


# --------------------------
# MAIN
# --------------------------
if __name__ == "__main__":
    app.run(debug=True)
