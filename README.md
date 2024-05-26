# doc2md
word to md 的最优复制粘贴实践 ， 只需一个函数轻松将Word转换成MD。如果节省了时间还请给个Star⭐！

# 文件转换
``` typescript
import * as fs from 'fs';
import * as path from 'path';
import * as mammoth from 'mammoth';
const TurndownService = require('turndown');
const gfm = require('turndown-plugin-gfm').gfm;

async function convertWordToMd(inputPath: string, outputPath: string): Promise<void> {
    try {
        const result = await mammoth.convertToHtml({ path: inputPath });
        const turndownService = new TurndownService();
        turndownService.use(gfm); // 使用 GitHub 风格的 Markdown 插件
        const markdown = turndownService.turndown(result.value);
        fs.writeFileSync(outputPath, markdown);
        console.log(`Converted ${inputPath} to ${outputPath}`);
    } catch (error) {
        console.error('Error during conversion:', error);
    }
}

function main() {
    const args = process.argv.slice(2);
    if (args.length !== 2) {
        console.log("Syntax: word2mdconverter <inputfile> <outputfile>");
        return;
    }
    const [inputPath, outputPath] = args;
    convertWordToMd(inputPath, outputPath);
}

main();

```
使用
```CMD
tsc [脚本文件名]
node [脚本文件名] [输入文件路径] [输出文件路径]

tsc word2mdconverter.ts
node word2mdconverter.js ./word.docx ./wordsa.md
```
# Base64函数使用
```JavaScript
// 输入: data 对象，包含 base64 编码的 DOCX 文件 (data.docx)。
// 输出: 解析后的 Markdown 文本字符串。
export const parseInocrMd= async (data) => {
  const rawData = toRaw(data); // 将响应式对象转换为普通对象

  if (!rawData || !rawData.docx) {
    console.error('Invalid response data');
    console.log('parseInocrDocx raw data', rawData); // 打印 rawData 以便调试
    return '';
  }

  const base64Docx = rawData.docx;

  const bytes = toByteArray(base64Docx);
  const arrayBuffer = bytes.buffer;
  const result = await mammoth.convertToHtml({ arrayBuffer: arrayBuffer });
  const turndownService = new TurndownService();
  turndownService.use(gfm); // 使用 GitHub 风格的 Markdown 插件
  const markdown = turndownService.turndown(result.value);
  return new TextDecoder().decode(new TextEncoder().encode(markdown));

};
```
如果你想导出....问问ChatGPT或把 new TextDecoder().decode(new TextEncoder().encode(markdown))的输出当成base64Content输入进下一个函数。
```
export function exportToMDFile(base64Content, fileName) {
      console.log(base64Content)
      const blob = new Blob([base64Content], { type: 'text/markdown;charset=utf-8' });
      saveAs(blob, fileName);
      console.log('Conversion completed');
}
```

如果这对你有用还请点个Star⭐！
