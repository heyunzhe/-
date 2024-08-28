## 根据服务器响应提示相应的弹窗
```js
document.addEventListener('DOMContentLoaded', () => {
    document.querySelector('form').addEventListener('submit', (event) => {
        event.preventDefault(); // 防止表单默认提交
        const formData = new URLSearchParams(new FormData(event.target));
        fetch('/log', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/x-www-form-urlencoded'
            },
            body: formData
        })
        .then(response => {
            if (response.ok) {
                alert("欢迎使用");
                window.location.href = '/success'; // 登录成功后跳转
            } else {
                alert('登录失败，请检查用户名和密码。');
            }
            })
        .catch(error => console.error('请求失败:', error));
        });
    });
```