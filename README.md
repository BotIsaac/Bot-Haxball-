var room = HBInit({
	roomName: "⚽Futsal x3⚽",
	maxPlayers: 14,
	noPlayer: true
});

room.setScoreLimit(3);
room.setTimeLimit(3);
room.setTeamsLock(true);

function updateAdmins() { 
  var players = room.getPlayerList();
  if ( players.length == 0 ) return; 
  if ( players.find((player) => player.admin) != null ) return; 
  room.setPlayerAdmin(players[0].id, true); 
}

room.onPlayerJoin = function(player) {
  updateAdmins();
	room.sendAnnouncement( " BEM VINDO " + player.name );
	room.sendAnnouncement( " ESCREVA !help PARA VER OS COMANDOS " );
}

room.onPlayerLeave = function(player) {
  updateAdmins();
	room.sendAnnouncement( " O JOGADOR " + player.name " SAIU DA SALA " );
}

room.onPlayerChat = function(player, message) {
	if ( message == "!admin xirimbau" ) {
	room.setPlayerAdmin(player.id, true);
	room.sendAnnouncement( " O JOGADOR " + player.name + " LOGOU COMO ADMINISTRADOR !! " );
	return false;
	}
	if ( message == "!limpar" && player.admin ) {
	room.clearBans();
	room.sendAnnouncement( " OS BANS FORAM LIMPOS !! " );
	return false;
	}
	if ( message == "!fechar" && player.admin ) {
	room.setPassword("treloso");
	room.sendAnnouncement( " A SALA FOI TRANCADA !! " );
	return false;
	}
	if ( message == "!abrir" && player.admin ) {
	room.setPassword();
	room.sendAnnouncement( " A SALA FOI ABERTA !! " );
	return false;
	}
	if ( message == "!discord" ) {
	room.sendAnnouncement( " ENTRE NO NOSSO DISCORD: " );
	return false;
	}
	if ( message == "!help" ) {
	room.sendAnnouncement( " COMANDOS: !limpar, !fechar, !abrir, !discord " );
	return false;
	}
}	
