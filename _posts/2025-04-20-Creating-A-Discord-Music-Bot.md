---
layout: post
published: true
categories:
  - TIL
title: '디스코드(Discord) 노래봇 만들기 🎵 (with Node.js)'
tags:
  - TIL
  - Discord
---

## 나는 어쩌다가 디스코드 노래봇을 만들게 되었는가...

때는 4월 17일 20시.. 공부를 하기 위해 책상에 앉아 디스코드에 접속하여 서버에서 기존에 사용하던 노래봇을 실행하려 했는데.. 유튜브 검색이 제대로 작동하지 않았다. 그래서 나는 다른 노래봇을 새롭게 불러와 서버에 실행했지만? 또 안됐다. 그렇게 한국어를 지원하는 어느정도 인지도가 있는 노래봇만 6개를 실행해봤다. 근데, 내가 원하는 유튜브로 검색하여 실행이 제대로 돌아가지 않았다.

<img width="519" alt="디스코드 노래봇 입장 채팅" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543059-d061e531-17d1-46e9-bd4f-c162037bf119.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042705Z&X-Amz-Expires=300&X-Amz-Signature=cc38f1e4105eaa841ce31c3d2c2d185cefb0885e68af14b1a38418300f8805ef&X-Amz-SignedHeaders=host" />

원하는대로 돌아가지 않아 약간 빡친(?) 나는, 너가 이기나 내가 이기나 해보자는 심보로 디스코드 노래봇을 어떻게 만드는지 찾아봤다. 어렵지 않으면 내가 만드는게 더 빠를 것 같다는 생각이 들었다. 오, 근데 간단하게 찾아보니 그리 어렵지 않고 주로 JS 혹은 Python으로 만드는 것 같았다. 직접 만드는게 진짜 더 좋겠는데...? 라는 생각에, 나는 계획에도 없던 디스코드 노래봇을 만들게 되었다 ^^!

## 🦐 나만의 디스코드 노래봇, 대하 만들기

나의 디스코드 노래봇의 이름은 '대하'이다. 이름이 대하인 이유는.. 내 닉네임 '소하'에서 '소'를 작을 소로 보고 큰 '대'로 바꿔 노래봇의 이름을 대하로 지어줬다. 나의 분신같은 친구니까. ^~^

내가 만들 디스코드 노래봇은 Node.js 환경에서 'discord.js'와 관련된 라이브러리를 활용하여 구현했다. 코드가 복잡한 것 하나 없이 매우 간단하게 1시간이면 만들 수 있다.

## 🎯 목표

일단 내가 만들 디스코드 노래봇의 목표는 다음과 같다.

- 디스코드 서버에서 명령어를 입력하면 **유튜브에서 노래 검색 및 재생** <- 유튜브 기반 검색과 재생이 매우 중요!!
- 기본적인 명령어: `/재생`, `/스킵`, `/대기열`, `/종료`
- 봇은 지정된 서버에서만 사용 가능 (내가 존재하는 서버에서만 사용할 것이기 때문)

## 🛠️ 준비물

- VSCode
- Node.js (LTS 버전 권장)
- Discord 계정 및 디스코드 개발자 포털에서 봇 생성 후 토큰 발급
- FFmpeg 설치

## 🔧 노래봇을 만들어 보자!

### 1. 디스코드 개발자 포털 설정

디스코드 개발자 포털 설정은 어렵지 않다! 아래 이미지와 글을 잘 읽고 따라오면 쉽게 생성할 수 있다.

#### 1. 새 애플리케이션 생성

[디스코드 개발자 포털](https://discord.com/developers/applications)로 이동하여 로그인을 하고, 새 애플리케이션 생성 버튼을 클릭한다.

<img width="812" alt="애플리케이션 생성" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435542957-4d202238-57ea-42fe-aa71-efb2fa16e3c4.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042842Z&X-Amz-Expires=300&X-Amz-Signature=40df6013e94452d0031b3aa848785e294821c6ffbb0540716c9d6a9ce2af8abf&X-Amz-SignedHeaders=host" />

애플리케이션의 이름을 작성하고 Create 버튼을 클릭하면,

<img width="812" alt="애플리케이션 이름 작성" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543622-b43f3e37-6962-4878-a345-eab4af54b6d3.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042950Z&X-Amz-Expires=300&X-Amz-Signature=c38a627ec2f99eb51d5f9a9a4b7b3d0856c1cfce3e1cb521f31321cecd22a9f0&X-Amz-SignedHeaders=host" />

애플리케이션 생성이 완료된다.

<img width="812" alt="애플리케이션 생성 완료" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543621-f604998d-6a71-4364-b4db-61a1c73b3952.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043010Z&X-Amz-Expires=300&X-Amz-Signature=b0351b030244343026286708bb3b4bc739fb497d3c721a5f2a6e652f847dbb98&X-Amz-SignedHeaders=host" />

Applicaion ID와 Public Key가 생성된 것을 확인할 수 있다. (이 두가지 정보는 이번 디스코드 노래봇 생성에는 사용되지 않음.)

#### 2. Bot Token 생성

위 이미지에서 왼쪽 “Bot”을 클릭하고 “Add Bot” 클릭하면 Bot이 생성된다.  
그리고 Bot 생성하고 Reset Token 버턴을 클릭하면 토큰이 만들어진다.  
이 토큰(Token)을 복사하여 저장해둔다. 혹은 `.env` 파일에 보관해두면 된다. (이건 뒤에서 또 설명)

<img width="812" alt="봇 토큰" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543869-130f9e1e-d4f2-45d6-9a82-d85037e49c78.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043104Z&X-Amz-Expires=300&X-Amz-Signature=d8f7ffe4d9613ff4b2d5543f73d167af42fd9eb5f1ba132ad3ed4a8eb958df3b&X-Amz-SignedHeaders=host" />

#### 3. “MESSAGE CONTENT INTENT” 옵션

Bot 섹션(Bot 페이지)에서 **Privileged Gateway Intents** 소제목 아래의 “MESSAGE CONTENT INTENT” 토글을 켜준다.

<img width="812" alt="message content intent 옵션" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435544081-1bb16d3d-1cd0-4941-b0f6-d4259515dd1d.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043229Z&X-Amz-Expires=300&X-Amz-Signature=a526a2df3f202502fade66be80749dac57014e8676b073bfc4dfa0e8423ede7d&X-Amz-SignedHeaders=host" />

Message Content Intent는 봇에게 Prefix 명령어(!play, !stop 등/messageCreate) 실행 시, message.content를 받으려면 켜야 한다. 단, 해당 옵션은 100개 이상 서버에 배포하려면 검증 및 승인 필요하다.

근데 여기서 잠깐! ✋ 슬래시 커맨드(/play)만 쓴다면, 이 Intent 없이도 명령 입력값을 받을 수 있다.

위 설명 방식은 messageCreate를 사용하지만 슬래시 커맨드를 사용한다면 interactionCreate를 사용하여 디스코드 내에서 자동완성 및 도움말을 제공할 수 있다. 단, messageCreate를 사용하여 구현하는 방식보다는 조금 더 만들기 복잡하다.

- **Prefix vs Slash 커맨드 비교**

  구분 | Prefix (messageCreate) | Slash (interactionCreate)
  구현 난이도 | 간단 (문자열 파싱) | 배포 스크립트 추가 필요
  사용자 UX | 접두사+명령어 암기 필요 | 자동완성·도움말 제공 ✨
  Intent 필요 여부 | Message Content Intent 필요 | 불필요
  서버 확장성 | 100개 이상 시 Intent 승인 필요 | 승인 절차 없이 배포 가능

  Slach 커맨드를 사용하는 것이 사용성을 고려했을 때 더 좋기 때문에.. 이건 추후 해당 방법으로 시도해보도록 하겠다! (일단 이번 글에서는 빠르게 구현이 목적이기 때문에 messageCreate를 사용)

#### 4. Bot을 서버로 초대하기 위한 URL 생성

OAuth2 섹션에 접속하여 아래 내용들을 켜준다.

- SCOPES: `bot`
- BOT PERMISSIONS:
  - `Connect`
  - `Speak`
  - `Send Messages`
  - `Embed Links`

URL 생성 범위를 Bot으로 설정

<img width="812" alt="OAuth2 bot 클릭" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543867-4e83cbcb-eb4e-476f-8ec5-8d72d6c4e846.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043121Z&X-Amz-Expires=300&X-Amz-Signature=6187b6d83f13283ddd23f73d8d2c8b5341cf60736b77557f930e948899f39f07&X-Amz-SignedHeaders=host" />

permisson 설정

<img width="812" alt="OAuth2 bot permission 선택" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543866-f5fb244d-d8eb-436b-b033-0797e83d4dd2.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043142Z&X-Amz-Expires=300&X-Amz-Signature=9cb5881df6fd747f9951812907e7588ed83dc1d8d3e5925cf362fcc16fa005df&X-Amz-SignedHeaders=host" />

위 설정들을 모두 완료했다면 생성된 ULR에 접속하여, 봇을 사용할 서버에 초대하면 끝!
<img width="812" alt="OAuth2 bot 초대 링크 복사" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435543868-44e04b5b-af90-4d43-9688-42b553dde88b.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T043155Z&X-Amz-Expires=300&X-Amz-Signature=24fa6aebef2acc1d86cc49d7cc368aea4f4eec648382ce1d603233cbcaf70f07&X-Amz-SignedHeaders=host" />

그러면 이렇게 디스코드 개발자 설정은 모두 완료되었다. 이제는 VScode를 실행하여 필요 패키지를 설치하고 코드를 작성하면 끝이다.

### 2. 프로젝트 초기 설정

폴더를 생성하고 프로젝트를 초기화한다.  
폴더 이름은 본인이 원하는 것으로 아무거나 해도 된다.

```bash
npm init -y
```

### 3. FFmpeg 설치

FFmpeg는 YouTube 스트림을 디스코드에서 재생 가능한 오디오 포맷(Opus)으로 변환해준다. 설치하지 않을 경우 음성 스트림 변환 과정에서 에러가 발생하기 때문에 꼭 설치해야 한다!

```bash
brew install ffmpeg
```

### 4. 필요 패키지 설치

```bash
npm install discord.js @discordjs/voice @distube/ytdl-core yt-search dotenv @discordjs/opus
```

- `discord.js`: 디스코드 API
- `@discordjs/voice`: 음성 채널 재생
- `@distube/ytdl-core`: YouTube 스트림 추출
- `yt-search`: 키워드 기반 검색
- `dotenv`: 환경변수 관리
- `@discordjs/opus`: Opus 인코딩

여기서 `@discordjs/opus`는 우리 코드에서 직접적으로 호출되지는 않는다. 그럼에도 설치를 꼭! 해줘야 한다. 디스코드 음성은 Opus 코덱을 사용해서 전송된다. `@discordjs/voice` 패키지는 내부적으로 Opus 스트림을 주고받기 때문에, Node.js 환경에서 Opus 처리를 해 줄 라이브러리가 반드시 있어야 한다. 이때, `@discordjs/opus`가 사용된다. 우리가 코드에서 직접 호출하지 않지만 `@discordjs/voice`가 자동으로 감지해서 사용한다.

그렇기에 우리 코드에서 사용하지 않는다고 해당 패키지를 삭제해서는 안된다!

### 5. 환경변수 설정 (.env)

```env
DISCORD_TOKEN=여기에_봇_토큰을_붙여넣기
PREFIX=/
GUILD_ID=여기에_서버_ID_입력
```

- `DISCORD_TOKEN`: 위 애플리케이션 생성 단계에서 2. Bot Token 생성에서 얻은 토큰을 넣어주면 된다.
- `GUILD_ID`: 봇을 허용할 서버 ID. 나의 경우에는 사용할 서버를 제한하는 조건을 넣을 것이기 때문에 해당 서버의 ID가 필요했다. (본인 프로필 설정 - 고급 - 개발자 모드 키고 사용할 서버 우클릭 - 서버 ID 복사하기 클릭)
- `PREFIX`: 명령어 접두사, 나는 `/` 로 사용했다. 다른 접두사도 사용해도 된다. (ex, `!`, `?` 등)

### 5. gitignore 설정 (Github에 올릴 경우에만)

```gitignore
node_modules/
.env
```

나의 경우에는 Github에 해당 코드를 업로드할 것이기 때문에 gitignore 설정을 해줬다. env 파일의 경우에는 다른 사람에게 공유되면 안되는 중요한 정보들이 들어있기 때문에 Github에 올라가지 않도록 꼭!!!! gitignore에 env 파일을 추가해줘야 한다.

### 6. index.js 주요 코드 설명

#### 1. 환경 변수 및 모듈 임포트

환경 변수를 로드하고 필요한 모듈을 불러옵니다.

```js
require('dotenv').config(); // .env 불러오기
const { Client, IntentsBitField, EmbedBuilder } = require('discord.js');
const { joinVoiceChannel, createAudioPlayer, createAudioResource, AudioPlayerStatus } = require('@discordjs/voice');
const ytdl = require('@distube/ytdl-core');
const ytSearch = require('yt-search');
```

#### 2. 클라이언트 설정

디스코드 봇 클라이언트를 생성하고, 필요한 인텐트를 설정한다.

```js
// 클라이언트 생성 (필요한 인텐트 활성화)
const client = new Client({
	intents: [
		IntentsBitField.Flags.Guilds, // 서버 접근
		IntentsBitField.Flags.GuildMessages, // 메시지 읽기
		IntentsBitField.Flags.MessageContent, // 메시지 내용
		IntentsBitField.Flags.GuildVoiceStates, // 음성 채널 상태
	],
});

const prefix = process.env.PREFIX;
const GUILD_ID = process.env.GUILD_ID;
const queueMap = new Map(); // 서버별 노래 대기열 저장
```

#### 3. 봇 로그인 이벤트

봇이 정상적으로 로그인되면 터미널에 메시지를 출력한다. 만약 터미널에 해당 메시지가 출력되지 않으면 정상적으로 봇이 디스코드에서 작동하지 않을 것이다! 그렇기에 해당 코드를 추가하는 것을 추천한다.

```js
// 봇 로그인
client.once('ready', () => {
	console.log(`✅ 로그인 완료! ${client.user.tag} (명령어 접두사: ${prefix})`);
});
```

#### 4. 노래 재생 함수 (`playSong`)

실제 음악 스트림을 생성하고 재생하며, 곡이 끝나면 자동으로 다음 곡을 재생한다.

- 노래를 정상적으로 재생할 경우

```js
// 곡 재생 함수
async function playSong(guildId, song) {
	const queue = queueMap.get(guildId);
	if (!song) {
		queue.connection.destroy();
		queueMap.delete(guildId);
		return;
	}
	// 스트림 생성
	const stream = ytdl(song.url, { filter: 'audioonly', highWaterMark: 1 << 25 });
	const resource = createAudioResource(stream);
	queue.player.play(resource);
	queue.connection.subscribe(queue.player);

	// 텍스트 알림
	queue.textChannel.send(`▶️ 재생: 🦐 ${song.title} 🦐`);

	// 노래 종료 후 처리
	queue.player.once(AudioPlayerStatus.Idle, () => {
		playSong(guildId, queue.songs.shift());
	});
}
```

#### 5. 명령어 처리 (`messageCreate` 이벤트)

- `/재생`: 유튜브 링크나 제목을 통해 음악을 검색 및 재생한다.
- `/스킵`: 현재 재생 중인 노래를 건너뛴다.
- `/대기열`: 현재 재생을 기다리는 노래 목록을 보여준다.
- `/종료`: 음악 재생을 종료하고 봇을 음성 채널에서 내보낸다.

```js
// 메시지 이벤트
client.on('messageCreate', async (message) => {
	if (message.author.bot) return; // 봇의 메시지는 무시
	if (!message.guild) return; // DM 등 무시
	if (message.guild.id !== GUILD_ID) {
		// 지정 서버 외 사용 제한
		return message.reply('❌ 이 서버에서는 사용할 수 없어요!');
	}
	if (!message.content.startsWith(prefix)) return;

	// 사용자의 명령어만 추출하는 코드
	const args = message.content.slice(prefix.length).trim().split(/ +/);
	const cmd = args.shift();

	// 재생 명령어: /재생 <키워드 or URL>
	if (cmd === '재생') {
		// ... 하단에 세부 코드가 따로 있음.
	}

	// 스킵 명령어: /스킵
	else if (cmd === '스킵') {
		// ... 하단에 세부 코드가 따로 있음.
	}

	// 목록 명령어: /대기열
	else if (cmd === '대기열') {
		// ... 하단에 세부 코드가 따로 있음.
	}

	// 종료 명령어: /종료
	else if (cmd === '종료') {
		// ... 하단에 세부 코드가 따로 있음.
	}
});
```

- 재생

  - 노래 제목이나 URL이 없을 경우: '⚠️ 노래 제목이나 URL을 입력해주세요!'
  - 사용자가 음성 채널에 들어가있지 않을 경우: '🎧 먼저 음성 채널에 들어가 주세요!'
  - 검색 결과가 없을 경우: '😥 검색 결과가 없습니다...'
  - 노래가 재생중인 상태일 때, 노래를 대기열에 정상적으로 추가했을 경우: `✅ 🦐 노래 제목🦐 를 대기열에 추가했어요!`

  ```js
  if (cmd === '재생') {
  	const query = args.join(' ');
  	if (!query) return message.reply('⚠️ 노래 제목이나 URL을 입력해주세요!');
  	const voiceChannel = message.member.voice.channel;
  	if (!voiceChannel) return message.reply('🎧 먼저 음성 채널에 들어가 주세요!');

  	// URL인지 확인
  	let songInfo, song;
  	if (ytdl.validateURL(query)) {
  		songInfo = await ytdl.getInfo(query);
  		song = { title: songInfo.videoDetails.title, url: songInfo.videoDetails.video_url };
  	} else {
  		// 키워드 검색
  		const { videos } = await ytSearch(query);
  		if (!videos.length) return message.reply('😥 검색 결과가 없습니다...');
  		song = { title: videos[0].title, url: videos[0].url };
  	}

  	// 대기열 관리
  	let queue = queueMap.get(message.guild.id);
  	if (!queue) {
  		// 1) 플레이어를 생성하고
  		const player = createAudioPlayer();
  		// 2) 에러 핸들러 등록 (스트림 에러 시 다음 곡으로 넘어가도록)
  		player.on('error', (error) => {
  			console.error('🔴 AudioPlayerError:', error);
  			// 다음 곡 재생 시도
  			playSong(message.guild.id, queue.songs.shift());
  		}); // 3) 큐 객체에 player를 포함시켜 저장
  		queue = {
  			voiceChannel,
  			textChannel: message.channel,
  			player,
  			songs: [],
  		};
  		queueMap.set(message.guild.id, queue);

  		// 채널 조인
  		const connection = joinVoiceChannel({
  			channelId: voiceChannel.id,
  			guildId: message.guild.id,
  			adapterCreator: message.guild.voiceAdapterCreator,
  		});
  		queue.connection = connection;
  		playSong(message.guild.id, queue.songs.shift() || song);
  	} else {
  		queue.songs.push(song);
  		return message.reply(`✅ 🦐 ${song.title} 🦐 를 대기열에 추가했어요!`);
  	}
  }
  ```

  **결과**  
  <img width="820" alt="재생" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435541786-55649117-5b6f-402b-9a24-63462877e467.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T041943Z&X-Amz-Expires=300&X-Amz-Signature=4ac0f7f0e1a22684a705b4da944da9f21693f0261dc3f76d88370649ff2627dd&X-Amz-SignedHeaders=host" />

- 스킵

  - 스킵할 노래가 없을 경우: '⚠️ 스킵할 노래가 없어요!''
  - 노래를 정상적으로 스킵할 경우: '⏭️ 노래를 스킵합니다!'

  ```js
  // 스킵 명령어: /스킵
  else if (cmd === '스킵') {
  	const queue = queueMap.get(message.guild.id);
  	// 1) 큐가 없거나, 2) 재생 중인 노래가 없고 대기열도 비어 있으면
  	if (!queue || (queue.player.state.status !== AudioPlayerStatus.Playing && queue.songs.length === 0)) {
  		return message.reply('⚠️ 스킵할 노래가 없어요!');
  	}
  	queue.player.stop();
  	return message.reply('⏭️ 노래를 스킵합니다!');
  }
  ```

  **결과**  
  <img width="820" alt="스킵" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435541950-baea9fd9-4a3f-417b-9169-e21a4be78d75.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042049Z&X-Amz-Expires=300&X-Amz-Signature=e99f1364ea94ca48f6caa81edfa056201fbd24885e4cb76bb502aee8f983c471&X-Amz-SignedHeaders=host" />

- 대기열

  - 대기열이 비어있을 경우: '📃 대기열이 비어있어요!'
  - 재생 대기열이 존재할 경우: '🎵 재생 대기열 ~~'

  ```js
  // 목록 명령어: /대기열
  else if (cmd === '대기열') {
  	const queue = queueMap.get(message.guild.id);
  	if (!queue || queue.songs.length === 0) {
  		return message.reply('📃 대기열이 비어있어요!');
  	}
  	const embed = new EmbedBuilder()
  		.setTitle('🎵 재생 대기열')
  		.setDescription(queue.songs.map((s, i) => `${i + 1}. ${s.title}`).join('\n'))
  		.setColor('#7E51F4');
  	return message.channel.send({ embeds: [embed] });
  }
  ```

  **결과**  
  <img width="820" alt="대기열" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435541876-6da0d31c-c948-4d3a-8c3b-7dd84bc6039d.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042015Z&X-Amz-Expires=300&X-Amz-Signature=351bee87c3efe748236114998a2fa82d285b4560ee68ec165f76b9cb6e198ebf&X-Amz-SignedHeaders=host" />

- 종료

  - 종료할 곡이 없을 경우: '⚠️ 종료할 곡이 없어요!'
  - 노래를 정상적으로 종료했을 경우: '👋 노래를 종료하고 🦐대하는 떠납니다!'

  ```js
  // 종료 명령어: /종료
  else if (cmd === '종료') {
  	const queue = queueMap.get(message.guild.id);

  	// 1) 큐가 없거나,
  	// 2) 재생 중인 곡이 없고(플레이어가 Playing 상태가 아니고),
  	//    대기열(songs)도 비어 있으면
  	if (!queue || (queue.player.state.status !== AudioPlayerStatus.Playing && queue.songs.length === 0)) {
  		return message.reply('⚠️ 종료할 곡이 없어요!');
  	}
  	queue.player.stop();
  	queue.connection.destroy();
  	queueMap.delete(message.guild.id);
  	return message.reply('👋 노래를 종료하고 🦐대하는 떠납니다!');
  }
  ```

  **결과**  
  <img width="820" alt="종료" src="https://github-production-user-asset-6210df.s3.amazonaws.com/77609591/435542037-86088ad1-073f-476f-983b-1b1882614a9a.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250421%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250421T042116Z&X-Amz-Expires=300&X-Amz-Signature=bae7b16f387eb75bd64076c1539bba61f10f97009d15cf8e13f8acf25cd5d740&X-Amz-SignedHeaders=host" />

## 🚨 내가 겪은 오류와 해결방법

### 🐞 문제 상황

npm run start를 통해 코드를 실행시키면 아래와 같은 경고가 발생했다.

```bash
npm WARN EBADENGINE Unsupported engine {
npm WARN EBADENGINE   required: { node: '20 || >=22' },
npm WARN EBADENGINE   current: { node: 'v21.0.0', npm: '9.5.1' }
}
```

해당 메시지는 설치하는 패키지들이 현재 사용 중인 Node.js 버전(**21.0.0**)을 공식적으로 지원하지 않는다는 의미이다.

### 💡 원인 분석

Node.js는 크게 두 가지 방식으로 버전을 관리한다.

- **짝수 버전(20, 22, 24 등)** 👉 장기 지원(LTS) 버전
- **홀수 버전(21, 23 등)** 👉 단기 지원, 실험적인 버전

많은 라이브러리와 패키지들은 **안정성이 보장된 LTS 버전만 공식 지원** 하도록 제한을 걸어둔다. 해당 경고는 Node.js의 현재 버전(**21.x**)이 짝수인 20버전이나 22 이상의 버전으로 설정된 지원 범위에서 벗어나기 때문에 나타난다.

### ✅ 해결 방법

가장 권장되는 방법은 **Node.js의 LTS 버전을 사용** 하는 것이다.

### 🔖 `nvm`으로 LTS 버전 설치하기

다음 명령어를 사용하면 손쉽게 버전을 바꿀 수 있다. (단, 해당 방법은 `nvm`으로 Node를 설치 했을 경우에만 가능하다.)

```bash
# LTS 최신 버전(v22.x) 설치
nvm install v22.14.0

# 설치된 LTS 버전 사용하기
nvm use v22.14.0
```

이렇게 하면 경고가 사라지고 패키지와의 호환성 문제가 해결된다!

## 📝 명령어 사용 방법

- `/재생 [유튜브 링크 또는 제목]`
- `/스킵`: 현재 재생중인 노래를 스킵
- `/대기열`: 재생 대기 중인 곡들을 리스트로 출력
- `/종료`: 봇의 노래 재생 종료

## 🖥️ 봇 실행하기

터미널에서 다음 명령어로 봇을 실행한다.

```bash
node index.js
# 또는 개발 시 nodemon 활용
npm install -g nodemon
nodemon index.js
```

두 명령어로 실행이 가능한데 매번 저렇게 터미널에 작성하기 귀찮으니 package.json의 스크립트에 만들어두자.

```json
//...
	"scripts": {
		"start": "node index.js",
		"dev": "nodemon index.js"
	},
//...
```

## 개발을 마치며

디스코드 노래봇 사용하다가 빡쳐서 만들었는데 어쩌다 보니 그닥 어렵지도 않고 만들고 사용하면서 지난번에 다른 사람들이 제작한 봇을 사용했을 때 보다 음질이 더 좋아서 아주 만족했다! 다음번에는 내가 서버를 열지 않아도 자동으로 서버가 계속 열려있을 수 있도록 ec2도 사용하고 디스코드에 노래봇을 사용할 때 명령어와 설명이 나올 수 있도록 interactionCreate를 사용해서 더 디벨롭해볼 예정이다.

그럼 아래에 index.js 풀 코드를 올리며 이만~ 오늘 글 끝!

## index.js 풀 코드

```js
require('dotenv').config(); // .env 불러오기
const { Client, IntentsBitField, EmbedBuilder } = require('discord.js');
const { joinVoiceChannel, createAudioPlayer, createAudioResource, AudioPlayerStatus } = require('@discordjs/voice');
const ytdl = require('@distube/ytdl-core');
const ytSearch = require('yt-search');

// 클라이언트 생성 (필요한 인텐트 활성화)
const client = new Client({
	intents: [
		IntentsBitField.Flags.Guilds, // 서버 접근
		IntentsBitField.Flags.GuildMessages, // 메시지 읽기
		IntentsBitField.Flags.MessageContent, // 메시지 내용
		IntentsBitField.Flags.GuildVoiceStates, // 음성 채널 상태
	],
});

const prefix = process.env.PREFIX;
const GUILD_ID = process.env.GUILD_ID;
const queueMap = new Map(); // 서버별 노래 대기열 저장

// 봇 로그인
client.once('ready', () => {
	console.log(`✅ 로그인 완료! ${client.user.tag} (명령어 접두사: ${prefix})`);
});

// 메시지 이벤트
client.on('messageCreate', async (message) => {
	if (message.author.bot) return; // 봇의 메시지는 무시
	if (!message.guild) return; // DM 등 무시
	if (message.guild.id !== GUILD_ID) {
		// 지정 서버 외 사용 제한
		return message.reply('❌ 이 서버에서는 사용할 수 없어요!');
	}
	if (!message.content.startsWith(prefix)) return;

	const args = message.content.slice(prefix.length).trim().split(/ +/);
	const cmd = args.shift();

	// 재생 명령어: /재생 <키워드 or URL>
	if (cmd === '재생') {
		const query = args.join(' ');
		if (!query) return message.reply('⚠️ 노래 제목이나 URL을 입력해주세요!');
		const voiceChannel = message.member.voice.channel;
		if (!voiceChannel) return message.reply('🎧 먼저 음성 채널에 들어가 주세요!');

		// URL인지 확인
		let songInfo, song;
		if (ytdl.validateURL(query)) {
			songInfo = await ytdl.getInfo(query);
			song = { title: songInfo.videoDetails.title, url: songInfo.videoDetails.video_url };
		} else {
			// 키워드 검색
			const { videos } = await ytSearch(query);
			if (!videos.length) return message.reply('😥 검색 결과가 없습니다...');
			song = { title: videos[0].title, url: videos[0].url };
		}

		// 대기열 관리
		let queue = queueMap.get(message.guild.id);
		if (!queue) {
			// queue = { voiceChannel, textChannel: message.channel, player: createAudioPlayer(), songs: [] };
			// 1) 플레이어를 생성하고
			const player = createAudioPlayer();
			// 2) 에러 핸들러 등록 (스트림 에러 시 다음 곡으로 넘어가도록)
			player.on('error', (error) => {
				console.error('🔴 AudioPlayerError:', error);
				// 다음 곡 재생 시도
				playSong(message.guild.id, queue.songs.shift());
			}); // 3) 큐 객체에 player를 포함시켜 저장
			queue = {
				voiceChannel,
				textChannel: message.channel,
				player,
				songs: [],
			};
			queueMap.set(message.guild.id, queue);

			// 채널 조인
			const connection = joinVoiceChannel({
				channelId: voiceChannel.id,
				guildId: message.guild.id,
				adapterCreator: message.guild.voiceAdapterCreator,
			});
			queue.connection = connection;
			playSong(message.guild.id, queue.songs.shift() || song);
		} else {
			queue.songs.push(song);
			return message.reply(`✅ 🦐 ${song.title} 🦐 를 대기열에 추가했어요!`);
		}
	}

	// 스킵 명령어: /스킵
	else if (cmd === '스킵') {
		const queue = queueMap.get(message.guild.id);
		// 1) 큐가 없거나, 2) 재생 중인 노래가 없고 대기열도 비어 있으면
		if (!queue || (queue.player.state.status !== AudioPlayerStatus.Playing && queue.songs.length === 0)) {
			return message.reply('⚠️ 스킵할 노래가 없어요!');
		}
		queue.player.stop();
		return message.reply('⏭️ 노래를 스킵합니다!');
	}

	// 목록 명령어: /대기열
	else if (cmd === '대기열') {
		const queue = queueMap.get(message.guild.id);
		if (!queue || queue.songs.length === 0) {
			return message.reply('📃 대기열이 비어있어요!');
		}
		const embed = new EmbedBuilder()
			.setTitle('🎵 재생 대기열')
			.setDescription(queue.songs.map((s, i) => `${i + 1}. ${s.title}`).join('\n'))
			.setColor('#7E51F4');
		return message.channel.send({ embeds: [embed] });
	}

	// 종료 명령어: /종료
	else if (cmd === '종료') {
		const queue = queueMap.get(message.guild.id);

		// 1) 큐가 없거나,
		// 2) 재생 중인 곡이 없고(플레이어가 Playing 상태가 아니고),
		//    대기열(songs)도 비어 있으면
		if (!queue || (queue.player.state.status !== AudioPlayerStatus.Playing && queue.songs.length === 0)) {
			return message.reply('⚠️ 종료할 곡이 없어요!');
		}
		queue.player.stop();
		queue.connection.destroy();
		queueMap.delete(message.guild.id);
		return message.reply('👋 노래를 종료하고 🦐대하는 떠납니다!');
	}
});

// 곡 재생 함수
async function playSong(guildId, song) {
	const queue = queueMap.get(guildId);
	if (!song) {
		queue.connection.destroy();
		queueMap.delete(guildId);
		return;
	}
	// 스트림 생성
	const stream = ytdl(song.url, { filter: 'audioonly', highWaterMark: 1 << 25 });
	const resource = createAudioResource(stream);
	queue.player.play(resource);
	queue.connection.subscribe(queue.player);

	// 텍스트 알림
	queue.textChannel.send(`▶️ 재생: 🦐 ${song.title} 🦐`);

	// 노래 종료 후 처리
	queue.player.once(AudioPlayerStatus.Idle, () => {
		playSong(guildId, queue.songs.shift());
	});
}

// 로그인 실행
client.login(process.env.DISCORD_TOKEN);
```
