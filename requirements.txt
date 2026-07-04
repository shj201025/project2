import streamlit as st
import random

# 페이지 설정
st.set_page_config(
    page_title="가위바위보 (3판 2선승)",
    page_icon="✊",
    layout="centered"
)

# 배경 스타일
st.markdown("""
<style>
.stApp {
    background-color: #0B1F4D;
    color: white;
}

h1, h2, h3, p {
    color: white;
}

div.stButton > button {
    width: 100%;
    height: 60px;
    font-size: 20px;
    border-radius: 10px;
}
</style>
""", unsafe_allow_html=True)

st.title("✊✋✌️ 가위바위보 (3판 2선승)")

# 세션 상태 (점수 저장)
if "user_score" not in st.session_state:
    st.session_state.user_score = 0
if "com_score" not in st.session_state:
    st.session_state.com_score = 0
if "round" not in st.session_state:
    st.session_state.round = 1
if "game_over" not in st.session_state:
    st.session_state.game_over = False

choices = {
    "가위": "✌️",
    "바위": "✊",
    "보": "✋"
}

# 점수판
st.subheader("📊 점수판")
col1, col2, col3 = st.columns(3)
col1.metric("🙋 나", st.session_state.user_score)
col2.metric("🤖 컴퓨터", st.session_state.com_score)
col3.metric("🔁 라운드", st.session_state.round)

st.write("---")

# 게임 종료 체크
if st.session_state.user_score == 2 or st.session_state.com_score == 2:
    st.session_state.game_over = True

if st.session_state.game_over:
    if st.session_state.user_score > st.session_state.com_score:
        st.success("🎉 최종 승리! 3판 2선승 성공!")
        st.balloons()
    else:
        st.error("😢 최종 패배... 컴퓨터 승리")

    if st.button("🔄 다시 시작"):
        st.session_state.user_score = 0
        st.session_state.com_score = 0
        st.session_state.round = 1
        st.session_state.game_over = False
    st.stop()

# 선택
user_choice = st.radio(
    "내 선택",
    list(choices.keys()),
    horizontal=True
)

if st.button("게임 시작! 🎮"):

    computer_choice = random.choice(list(choices.keys()))

    st.subheader(f"📢 {st.session_state.round} 라운드 결과")

    col1, col2 = st.columns(2)

    with col1:
        st.markdown("### 😀 나")
        st.markdown(f"# {choices[user_choice]}")
        st.write(user_choice)

    with col2:
        st.markdown("### 🤖 컴퓨터")
        st.markdown(f"# {choices[computer_choice]}")
        st.write(computer_choice)

    # 판정
    if user_choice == computer_choice:
        st.info("🤝 무승부!")

    elif (
        (user_choice == "가위" and computer_choice == "보") or
        (user_choice == "바위" and computer_choice == "가위") or
        (user_choice == "보" and computer_choice == "바위")
    ):
        st.success("🎉 당신 승리!")
        st.session_state.user_score += 1

    else:
        st.error("😢 컴퓨터 승리!")
        st.session_state.com_score += 1

    # 라운드 증가
    st.session_state.round += 1
