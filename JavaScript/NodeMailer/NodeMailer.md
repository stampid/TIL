# NodeMailer

## nodeMailer란?

nodeMailer는 Node.js 환경에서 email을 보내주도록 도와주는 모듈이다.  
메일은 전송할 수 있고 메일의 내용을 HTML 문서로 사용, 혹은 파일 첨부 가능도 가능하다.

## 사용 방법

```javascript
npm install nodemailer
npm install nodemailer-sendgrid-transport
```

- 먼저 node-mailer 모듈을 설치한다.
- transport로 사용할 sendgrid 모듈을 섫치한다.

```javascript
// 랜덤한 문자를 만들기 위한 함수이다.
export const generateSecret = () => {
  const randomNumber = Math.floor(Math.random() * adjectives.length);
  return `${adjectives[randomNumber]} ${nouns[randomNumber]}`;
};

// 실제로 메일을 보내주기 위한 함수
const sendMail = email => {
  const options = {
    auth: {
      api_user: process.env.SENDGRID_USERNAME,
      api_key: process.env.SENDGRID_PASSWORD
    }
  };

  const client = nodemailer.createTransport(sgTransport(options));
  return client.sendMail(email);
};

// 메일의 양식을 지정하는 함수
export const sendSecretMail = (address, secret) => {
  const email = {
    from: "gaegosu@naver.com",
    to: address,
    subject: "SingUp Secret for Gaegosu",
    html: `Hello Your login secret it <h1>${secret}</h1>.<br/> Copy paste on the Web to Sign Up`
  };

  return sendMail(email);
};
```

- nodemailer의 메소드인 createTransport를 이용해 발송할 메일의 객체를 생성한다.
- 만들어진 객체의 sendMail 메소드를 통해 미리 만들어둔 메일 양식 데이터를 통해 메일을 발송할 수 있습니다.
