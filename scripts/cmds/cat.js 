const axios = require("axios");

module.exports = {
  config: {
    name: "cat",
    aliases: [],
    version: "1.0",
    author: "NZ R",
    category: "Chat~AI",
    cooldown: 0,
    role: 0,
    guide: {
      en: "{p}chat hi\nfor initiating conversation: {p}chat hi"
    }
  },
  nehalovesMetaApiRequest: async function (question) {
    try {
      const res = await axios.post(
        'https://api.simsimi.vn/v2/simtalk',
        new URLSearchParams({
          'text': question,
          'filter': true,
          'filterRatio': 60,
          'lc': 'bn'
        })
      );

      if (res.status > 200)
        throw new Error(res.data.success);

      return res.data.message;
    } catch (error) {
      throw error;
    }
  },
  nehalovesMetaHandleCommand: async function ({ args, message, isError }) {
    try {
      const text = args.join(' ');

      if (!text.trim()) {
        message.reply('Please type something 😾.', (err, info) => {
          global.GoatBot.onReply.set(info.messageID, {
            commandName: this.config.name,
            messageID: info.messageID,
            author: message.senderID,
            isError: true
          });
        });
        return;
      }

      if (args.length === 1 && text.match(/[\u{1F300}-\u{1F6FF}]/u)) {
        message.reply('Ugh, just emojis? Try using words! 😒', (err, info) => {
          global.GoatBot.onReply.set(info.messageID, {
            commandName: this.config.name,
            messageID: info.messageID,
            author: message.senderID,
            isError: true
          });
        });
        return;
      }

      try {
        const result = await this.nehalovesMetaApiRequest(text);
        message.reply(result, (err, info) => {
          global.GoatBot.onReply.set(info.messageID, {
            commandName: this.config.name,
            messageID: info.messageID,
            author: message.senderID
          });
        });
      } catch (error) {
        console.error(error);
        message.reply(this.getRandomErrorMessage(), (err, info) => {
          global.GoatBot.onReply.set(info.messageID, {
            commandName: this.config.name,
            messageID: info.messageID,
            author: message.senderID,
            isError: true
          });
        });
      }
    } catch (error) {
      message.reply(this.getRandomErrorMessage(), (err, info) => {
        global.GoatBot.onReply.set(info.messageID, {
          commandName: this.config.name,
          messageID: info.messageID,
          author: message.senderID,
          isError: true
        });
      });
    }
  },
  onStart: function ({ args, message }) {
    return this.nehalovesMetaHandleCommand({ args, message, isError: false });
  },
  onReply: function ({ args, message }) {
    const { isError } = global.GoatBot.onReply.get(message.reply_to_message_id) || {};
    return this.nehalovesMetaHandleCommand({ args, message, isError: !!isError });
  },
  getRandomErrorMessage: function () {
    const errorMessages = [
      'Chup thako toh valo lagtese nah 😔',
      'Areeh kotha bolio na.!! Nahole gali dibo  😼',
      'Tumi toh pocha 😾',
      'Hehe Ami ghumabo tata 😼💀',
      'Mara Khaw 😪',
      'Fak tomar kotha porte gelei error ashe 😞👌',
      'Ajke error ashlo bole gali dite parlam nah 💀'
    ];
    return errorMessages[Math.floor(Math.random() * errorMessages.length)];
  }
};
