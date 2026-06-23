# Code-Free_discord
function msgDiscord(message, imageUrl) {
  const discord = "https://discordapp.com/api/webhooks/1518763533737787392/0fVIHBy3VK9abd0U8E6UOByMK2WNxLaKlFwYUSi5lT9nurG08QFMFhVitg7rlO-Z14SA";
  const payload = {
    'content': message,
    'embeds': [
      {
        'image': {
          'url': imageUrl
        }
      }
    ]
  };
  const options = {
    'method': 'post',
    'contentType': 'application/json',
    'payload': JSON.stringify(payload)
  };

  UrlFetchApp.fetch(discord, options);
}
