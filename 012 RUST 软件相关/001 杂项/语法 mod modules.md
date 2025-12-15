在main.rs中（和每个.rs文件），可以link到多个modules；
mod editor; 则本文件夹有editor.rs文件（或editor文件夹有mod.rs文件）。
在editor.rs中，可以link到多个modules，如mod terminal;
然而，从main.rs无法访问缺少pub的terminal