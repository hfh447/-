# -<img width="2304" height="1491" alt="image" src="https://github.com/user-attachments/assets/8c34797e-7c3c-4221-aa97-41b00e42e8e2" />
'''
《人工智能导论》作业02 - Chatbot 示例
功能：调用 DeepSeek R1/Distill 模型进行对话
'''

import os
from openai import OpenAI

# --- 配置区 ---
# 请前往 https://platform.deepseek.com/ 获取你的 API Key
# 为了安全，建议将 API Key 存在环境变量中，或者直接写在这里（提交时记得删除或脱敏）
API_KEY = "sk-你的-deepseek-api-key-here" 

# 初始化客户端
# base_url: DeepSeek 的兼容 OpenAI 接口地址
client = OpenAI(api_key=API_KEY, base_url="https://api.deepseek.com")

def chat_with_deepseek(prompt):
    """
    发送请求到 DeepSeek 模型并打印回复
    """
    try:
        # 创建聊天完成请求
        # model: 可以选择 deepseek-chat (通用版) 或 deepseek-reasoner (推理版)
        response = client.chat.completions.create(
            model="deepseek-chat", 
            messages=[
                {"role": "system", "content": "你是一个乐于助人的AI助手，由DeepSeek开发。"},
                {"role": "user", "content": prompt}
            ],
            # 可选参数
            temperature=0.7, # 创造性，0-2之间
            max_tokens=1024 # 最大回复长度
        )
        
        # 提取并打印模型回复
        reply = response.choices[0].message.content
        print(f"🤖 DeepSeek: {reply}")
        
    except Exception as e:
        print(f" 调用失败: {e}")
        print("请检查你的API Key是否正确，或者网络是否通畅。")

if __name__ == "__main__":
    print("🚀 DeepSeek Chatbot 已启动 (输入 'quit' 退出)")
    
    while True:
        user_input = input("\n👤 你: ")
        if user_input.lower() == 'quit':
            break
        chat_with_deepseek(user_input)
