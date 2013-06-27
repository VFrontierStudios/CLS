CLS
===

A Minecraft Chat and Inv Cleaner!


The code is as follows: Made in Eclipse


package me.kingdomscraft.cls;

import java.io.File;
import java.util.logging.Logger;

import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.OfflinePlayer;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.configuration.file.FileConfiguration;
import org.bukkit.entity.Player;
import org.bukkit.plugin.PluginManager;
import org.bukkit.plugin.java.JavaPlugin;

public class CLS extends JavaPlugin {
  public static Logger logger=Logger.getLogger ("Minecraft");
	public static CLS pluign;
	public static FileConfiguration config;
	public static File cfile;
	private static String version;
	private static boolean toggle;
	private Permissions perms;
	
	@Override
	public void onEnable () {
		version="2.1";
		toggle=true;
		config=getConfig ();
		config.options().copyDefaults(true);
		saveConfig();
		cfile=new File (getDataFolder(), "config.yml");
		PluginManager pm=getServer().getPluginManager();
		perms=new Permissions();
		pm.addPermission(perms.canPerformCLS);
		getServer().getScheduler().scheduleSyncRepeatingTask(this, new Runnable(){
			@Override
			public void run(){
				if (toggle) {
					for(Player p:Bukkit.getOnlinePlayers()){
						for(int x=0;x<121;x++){
							p.sendMessage("");
						}
					}
					Bukkit.broadcastMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + ChatColor.AQUA+" The chat was cleared by " + ChatColor.LIGHT_PURPLE + "Console" + ChatColor.AQUA + "!");
				}
			}
		}, 0L, 2160000);
		logger.info("CLS Version "+ version + " has been enabled!");
	}
	@Override
	public void onDisable () {
		PluginManager pm=getServer().getPluginManager();
		pm.removePermission(perms.canPerformCLS);
		logger.info("ClS has been disabled!");
	}
	@Override
	public boolean onCommand(CommandSender sender, Command command,
			String label, String[] args) {
		if(sender instanceof Player) {
			Player player=(Player)sender;
			if (args.length==0) {
				if (label.equalsIgnoreCase("CLS")&&(player.hasPermission(perms.canPerformCLS)||player.hasPermission(perms.canPreformAll))) {
					if (toggle) {
						for(Player p:Bukkit.getOnlinePlayers()){
							for(int x=0;x<121;x++){
								p.sendMessage("");
							}
						}
						Bukkit.broadcastMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + ChatColor.AQUA+" The chat was cleared by "+player.getDisplayName()+ChatColor.AQUA+"!");
						return true;
					}else{
						player.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + " CLS is turned off!");
					}
				}
			}else if(args.length==1&&(player.hasPermission(perms.canPerformToggle)||player.hasPermission(perms.canPreformAll))){
				if(args[0].equalsIgnoreCase("toggle")){
					String state="off";
					if(toggle){
						toggle=false;
					}else{
						toggle=true;
						state="on";
					}
					player.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + ChatColor.AQUA+" CLS has been turned "+state+ChatColor.AQUA+"!");
					return true;
				}else if(args[0].equalsIgnoreCase("me")&&(player.hasPermission(perms.canPerformMe)||player.hasPermission(perms.canPreformAll))){
					player.getInventory().clear();
					player.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.AQUA + "CLS has cleaned your inventory");
				}else if(args[0].equalsIgnoreCase("all")&&(player.hasPermission(perms.canPerformAll)||player.hasPermission(perms.canPreformAll))){
					for(Player p:Bukkit.getOnlinePlayers()){
						p.getInventory().clear();
					    p.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.AQUA + "A server admin with a sick mind has cleaned your inventory");
					}
					}else if(args.length>1){
				player.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + ChatColor.RED+" Too many arguments!");
				return false;
			}
			}else{
				player.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.GREEN + ChatColor.RED+" You do not permission to use this command");
				return false;
			}
			}else if(args[0].equalsIgnoreCase("Info")&&(player.hasPermission(perms.canPerformInfo)||player.hasPermission(perms.canPerformInfo))){
				p.sendMessage(ChatColor.GOLD+"["+ChatColor.AQUA+ "CLS"+ ChatColor.GOLD +"]" + ChatColor.RED + "Version 2.1");	
		}
		return false;
	
	}	
}

