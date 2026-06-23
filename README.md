# Code-Free_discord
function doGet() {
  return HtmlService.createTemplateFromFile('Index').evaluate()
  .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
  .setTitle('Epic Coding Channel')
  .setFaviconUrl('https://cdn.jsdelivr.net/gh/EPICCODING17/image/Logo-EicCoding.png')
  .addMetaTag('viewport', 'width=device-width, initial-scale=1')
}

const getURL = () => {
  return ScriptApp.getService().getUrl();
}

const include = (filename) => {
  return HtmlService.createHtmlOutputFromFile(filename).getContent()
}

function msgDiscord(message, imageUrl) {
  const discord = "";
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

function getDataCatalog(){
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Data');
  const data = sheet.getDataRange().getDisplayValues().slice(1);
  //Logger.log(data)
  return data;
}

function generateID(currentID) {
  const prefix = 'PD-';
  const number = currentID.toString().padStart(5, '0');
  return `${prefix}${number}`;
}

function addAllData(obj) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Data');
  const lastRowID = sheet.getLastRow();
  const codeID = generateID(lastRowID);
  const documentFolder = DriveApp.getFolderById("1YXPurKVABwhssCjJWFeoPUQj5A1Eevy9");
  let ucfile = "";
  if(obj.myDataImg.length > 0){
    let file = documentFolder.createFile(obj.myDataImg).getId();
    ucfile = "https://lh3.googleusercontent.com/d/" + file;
  }
  const rowData = [codeID, obj.data1, obj.data2, obj.data3, ucfile];
  sheet.appendRow(rowData);

  const msg = `แจ้งเตือน` +
              `\nKey: ${codeID}` +   
              `\nรายการ: ${obj.data1} ${obj.data2} ${obj.data3}`;
  msgDiscord(msg, ucfile);

  return sheet.getRange("A2:E" + sheet.getLastRow()).getValues();
}
