

### I. 프로젝트 개요(미션 목표 요약)
### II. 실행 환경(OS/쉘/터미널, Docker 버전, Git 버전)
### III. 수행 항목 체크리스트(터미널/권한/Docker/Dockerfile/포트/볼륨/Git/GitHub)
### IV. 검증 방법(어떤 명령으로 무엇을 확인했는지) + 결과 위치 링크
### V. 트러블슈팅
    1. 
        (1) 문제:
        (2) 원인/가설:
        (3) 확인:
        (4) 해결/대안:
    2. 
        (1) 문제:
        (2) 원인/가설:
        (3) 확인:
        (4) 해결/대안:




```
import json # JSON 파일(세이브 파일)을 읽고 쓰기 위해 파이썬에 내장된 도구를 가져옵니다.
from question_model import Question # question_model.py 파일에서 Question 클래스(큐카드 틀)를 가져옵니다.
from quiz_brain import QuizBrain # quiz_brain.py 파일에서 QuizBrain 클래스(퀴즈 진행자)를 가져옵니다.
from quiz_data import question_data # quiz_data.py 파일에서 기본 퀴즈 데이터(작가 노트)를 가져옵니다.

def load_data(): # 게임의 진행 상황(세이브 파일)을 불러오는 함수를 정의합니다.
    try: # 먼저 아래의 코드를 시도해 봅니다. (파일이 존재하는지 확인하는 안전장치)
        # "state.json" 파일을 읽기 모드("r")로 엽니다. 한글이 깨지지 않게 utf-8 인코딩을 사용합니다.
        with open("state.json", "r", encoding="utf-8") as f:
            return json.load(f) # JSON 파일의 글자들을 파이썬이 다루기 쉬운 '딕셔너리'로 변환하여 반환합니다.
            
    except FileNotFoundError: # 만약 파일을 찾을 수 없다는 에러가 발생하면 프로그램이 꺼지지 않고 이쪽으로 넘어옵니다.
        # 파일이 없으므로(첫 실행), 최고 점수 0점과 기본 퀴즈 데이터를 담은 초기 상태를 반환합니다.
        return {"high_score": 0, "questions": question_data}

def save_data(data): # 변경된 게임 데이터(점수, 추가된 퀴즈 등)를 파일에 저장하는 함수입니다.
    # "state.json" 파일을 쓰기 모드("w")로 엽니다. (기존 내용이 있으면 덮어씁니다)
    with open("state.json", "w", encoding="utf-8") as f:
        # 파이썬 딕셔너리(data)를 JSON 파일(f)에 씁니다.
        # ensure_ascii=False: 한글이 유니코드 기호로 깨지지 않게 합니다.
        # indent=4: 파일 내용을 사람이 읽기 좋게 4칸 들여쓰기로 예쁘게 정렬해 줍니다.
        json.dump(data, f, ensure_ascii=False, indent=4)

def main(): # 프로그램의 메인 흐름을 담당하는 총괄 PD 함수입니다.
    data = load_data() # 프로그램 시작 시, 가장 먼저 세이브 데이터를 불러와 'data' 변수에 저장합니다.
    
    while True: # 사용자가 '종료'를 선택할 때까지 메뉴를 계속 보여주기 위해 무한 반복을 시작합니다.
        
        # 사용자에게 보여줄 메뉴판을 화면에 출력합니다.
        print("\n=== 📚 세계 문학 퀴즈 프로그램 ===")
        print("1. 퀴즈 풀기")
        print("2. 퀴즈 등록")
        print("3. 퀴즈 목록")
        print("4. 최고 점수 확인")
        print("5. 종료")
        
        # 사용자로부터 원하는 메뉴 번호를 입력받아 'choice' 변수에 문자열(글자)로 저장합니다.
        choice = input("메뉴를 선택하세요: ")

        if choice == "1": # 사용자가 1번(퀴즈 풀기)을 선택했을 때 실행됩니다.
            
            # [핵심] data["questions"]에 있는 글자 데이터를 하나씩 꺼내어 Question 객체(빳빳한 큐카드)로 변환한 뒤, 리스트로 묶습니다.
            question_bank = [Question(q["text"], q["answer"]) for q in data["questions"]]
            
            # 만들어진 큐카드 뭉치(question_bank)를 진행자(QuizBrain)에게 넘겨주며 진행자 객체를 생성합니다.
            quiz = QuizBrain(question_bank)
            
            while quiz.still_has_questions(): # 진행자가 남은 문제가 있는지 확인합니다. (있으면 True 반환)
                quiz.next_question() # 남은 문제가 있다면 다음 문제를 출제합니다.
            
            # 모든 문제를 다 풀고 while문이 끝나면, 최종 점수를 출력합니다.
            print(f"\n게임 종료! 최종 점수: {quiz.score}/{quiz.question_number}")
            
            if quiz.score > data["high_score"]: # 방금 얻은 점수가 기존 최고 점수보다 높다면?
                data["high_score"] = quiz.score # 최고 점수를 새로운 점수로 덮어씁니다.
                print(f"🎊 최고 점수 갱신: {data['high_score']}점!") # 축하 메시지를 출력합니다.
                save_data(data) # 갱신된 최고 점수를 잃어버리지 않게 파일에 저장합니다.

        elif choice == "2": # 사용자가 2번(퀴즈 등록)을 선택했을 때 실행됩니다.
            text = input("새로운 퀴즈 내용을 입력하세요: ") # 새 문제의 내용을 입력받습니다.
            answer = input("정답을 입력하세요 (True/False): ") # 새 문제의 정답을 입력받습니다.
            
            # 입력받은 문제와 정답을 딕셔너리 형태로 만들어 기존 질문 리스트 맨 끝에 추가(append)합니다.
            data["questions"].append({"text": text, "answer": answer})
            save_data(data) # 문제가 추가되었으니 파일에 다시 저장합니다.
            print("✅ 퀴즈가 성공적으로 등록되었습니다.") # 성공 메시지를 출력합니다.

        elif choice == "3": # 사용자가 3번(퀴즈 목록)을 선택했을 때 실행됩니다.
            print("\n--- 등록된 퀴즈 목록 ---")
            # enumerate를 사용해 1번부터 번호를 매기며 퀴즈 목록을 반복 출력합니다.
            for i, q in enumerate(data["questions"], 1):
                print(f"{i}. {q['text']} (정답: {q['answer']})") # 번호. 문제 (정답: O/X) 형태로 출력합니다.

        elif choice == "4": # 사용자가 4번(최고 점수 확인)을 선택했을 때 실행됩니다.
            print(f"\n🏆 현재 최고 점수: {data['high_score']}점") # 딕셔너리에 저장된 최고 점수를 출력합니다.

        elif choice == "5": # 사용자가 5번(종료)을 선택했을 때 실행됩니다.
            print("프로그램을 종료합니다. 안녕히 가세요!") # 종료 메시지를 출력합니다.
            break # 무한 반복(while True)을 깨고(break) 루프를 탈출하여 프로그램을 완전히 끝냅니다.
            
        else: # 사용자가 1~5번이 아닌 다른 값(예: "안녕", "6")을 입력했을 때 실행됩니다.
            # 경고 메시지를 띄우고 다시 while문의 처음(메뉴 출력)으로 돌아갑니다.
            print("잘못된 선택입니다. 다시 입력해주세요.") 

# 이 파일이 다른 파일에 의해 불려온(import) 것이 아니라, 직접 실행되었을 때만 아래 코드를 실행하라는 파이썬의 규칙입니다.
if __name__ == "__main__":
    main() # 총괄 PD인 main() 함수를 호출하여 프로그램을 시작합니다.

```