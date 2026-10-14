# Reproduction of Streamlit issue #7556

Issue: "With st.file_uploader and st.chat_input, first message disappears"
Fix PR: #7641, released in Streamlit 1.28.2

## Environment
- OS: macOS (MacBook Air)
- Python: 3.11 (conda environment)
- Streamlit: 1.28.0 (buggy) and 1.28.2 (fixed)
- Note: pyarrow had to be pinned to 14.0.2, because the newest pyarrow needs NumPy 2 and Streamlit 1.28.0 uses NumPy 1.x

## App used (from the issue)
```python
import streamlit as st

st.set_page_config(page_title="Bug")

uploaded_files = st.sidebar.file_uploader(
    label="Upload", type=["pdf"], accept_multiple_files=True
)

st.chat_message("assistant").write("Ask me anything")

if user_query := st.chat_input(placeholder="Ask me anything!"):
    st.chat_message("user").write(user_query)
```

## Steps
1. Run `streamlit run app.py` and load a fresh page
2. Type `hello` in the chat input and press Enter
3. Type `hello again` and press Enter

## Results
| Version | Result |
|---|---|
| 1.28.0 | First message did not stay visible; second message appeared |
| 1.28.2 | First message stayed on screen (bug fixed) |
