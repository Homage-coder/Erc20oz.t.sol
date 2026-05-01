// SPDX-License-Identifier: MIT
pragma solidity 0.8.33;

import {Test} from "forge-std/Test.sol";
import {ERC20OZ} from "../src/ERC20OZ.sol";

contract ERC20OZTest is Test {
    ERC20OZ public token;

    address public owner;
    address public user1 = address(1);
    address public user2 = address(2);

    string public constant NAME = "Ethereum";
    string public constant SYMBOL = "ETH";
    uint256 public constant INITIAL_SUPPLY = 10_000_000_000;

    function setUp() public {
        owner = address(this);
        token = new ERC20OZ(NAME, SYMBOL);
    }

    function testInitialSupplyMintedToContract() public view {
        assertEq(token.balanceOf(address(token)), INITIAL_SUPPLY);
        assertEq(token.totalSupply(), INITIAL_SUPPLY);
    }

    function testTokenMetadata() public view {
        assertEq(token.name(), NAME);
        assertEq(token.symbol(), SYMBOL);
    }

    function testOwnerCanMintTokens() public {
        uint256 amountToMint = 1_000 ether;

        token.mint(amountToMint, user1);

        assertEq(token.balanceOf(user1), amountToMint);
        assertEq(token.totalSupply(), INITIAL_SUPPLY + amountToMint);
    }

    function testNonOwnerCannotMint() public {
        vm.prank(user1);
        vm.expectRevert();
        token.mint(1_000 ether, user2);
    }

    function testOwnerCanBurnTokens() public {
        uint256 amountToBurn = 1_000_000;
        uint256 initialBalance = token.balanceOf(address(token));

        token.burn(amountToBurn);

        assertEq(token.balanceOf(address(token)), initialBalance - amountToBurn);
        assertEq(token.totalSupply(), INITIAL_SUPPLY - amountToBurn);
    }

    function testNonOwnerCannotBurn() public {
        vm.prank(user1);
        vm.expectRevert();
        token.burn(1_000);
    }

    function testTransferBetweenUsers() public {
        uint256 mintAmount = 5_000;
        uint256 transferAmount = 2_000;

        token.mint(mintAmount, user1);

        vm.prank(user1);
        token.transfer(user2, transferAmount);

        assertEq(token.balanceOf(user1), mintAmount - transferAmount);
        assertEq(token.balanceOf(user2), transferAmount);
    }

    function testOnlyOwnerIsDeployer() public view {
        assertEq(token.owner(), owner);
    }
}
