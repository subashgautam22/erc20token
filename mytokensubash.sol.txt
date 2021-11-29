// SPDX-License-Identifier: MIT
pragma solidity ^0.6.5;

interface IERC20 {
     function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view  returns (uint256);

    function transfer(address recipient, uint256 amount) external  returns (bool);
    
    function mint(address recipient, uint256 amount) external  returns (bool);

}

 library SafeMath {
    function add(uint a, uint b) public pure returns (uint c) {
        c = a + b;
        require(c >= a);
    }
    function sub(uint a, uint b) public pure returns (uint c) {
        require(b <= a); c = a - b; } 
    function Mul(uint a, uint b) public pure returns (uint c) { c = a * b; 
        require(a == 0 || c / a == b); } 
    function Div(uint a, uint b) public pure returns (uint c) { 
        require(b > 0);
        c = a / b;
    }
}

contract Mysubash is IERC20{
    
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping(address => bool) private minters;
    address public owner;
    string private _name;
    string private _symbol;
    uint8 private _decimals;
    uint256 private _totalSupply;

    constructor() public {
        _symbol = "SUB";
        _name = "SUBASH GAUTAM";
        _decimals = 18;
        _totalSupply = 1000000000000000000000000;
        minters[msg.sender] = true;
    } 
    
    modifier onlyOwner(){
        require(msg.sender == owner);
        _;
    }
    
    modifier onlyMinters(){
        require(minters[msg.sender]);
        _;
    }

    
    function name() public view returns (string memory) {
        return _name;
    }

    
    function symbol() public view returns (string memory) {
        return _symbol;
    }
    
    
    function getStatus() public view returns (bool) {
        return minters[msg.sender];
    }
    
   
    function totalSupply() public view  override  returns (uint256) {
        return _totalSupply;
    }

    
    function balanceOf(address account) public  view override  returns (uint256) {
        return _balances[account];
    }

    
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    
    function mint(address recipient, uint256 amount) public override  onlyMinters returns (bool) {
        _mint(recipient, amount);
        return true;
    }
    
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(amount);
       
    }

    function _mint(address account, uint256 amount) public onlyMinters {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        
    }

    function _burn(address account, uint256 value) internal {
        require(account != address(0), "ERC20: burn from the zero address");
        _balances[account] = _balances[account].sub(value);
        _totalSupply = _totalSupply.sub(value);
        
       
    }
    

    
}